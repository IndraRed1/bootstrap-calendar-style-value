import { createComponentFactory, Spectator } from '@ngneat/spectator/jest';
import { AttributionTableComponent } from './attribution-table.component';
import { MockComponent } from 'ng-mocks';
import { HeroiconComponent } from '@frk/ng-ui-core';
import { PRODUCTS_SERVICE } from '@frk/ng-ui-extra';
import { LoggerService } from '@frk/shared-utils';
import { TranslateService } from '@frk/shared-utils';
import { createLoggerMockService } from '@frk/shared-utils/mocks';
import * as dayjs from 'dayjs';

describe('AttributionTableComponent', () => {
  let spectator: Spectator<AttributionTableComponent>;
  let component: AttributionTableComponent;

  const mockProductsService = {} as any;
  const mockLoggerService = createLoggerMockService();
  const translateService = new TranslateService(mockLoggerService, {});

  const createComponent = createComponentFactory({
    component: AttributionTableComponent,
    declarations: [MockComponent(HeroiconComponent)],
    providers: [
      { provide: PRODUCTS_SERVICE, useValue: mockProductsService },
      { provide: LoggerService, useFactory: createLoggerMockService },
      { provide: TranslateService, useValue: translateService },
    ],
  });

  beforeEach(() => {
    spectator = createComponent();
    component = spectator.component;
  });

  it('should create the component', () => {
    expect(component).toBeTruthy();
  });

  it('should format selectedAsOfDate and update tableData on ngOnChanges', () => {
    // Mock observables
    const selectedAsOfDateMock = jasmine.createSpy('selectedAsOfDate')
      .and.returnValue('2024-01-01');
    selectedAsOfDateMock.set = jasmine.createSpy('selectedAsOfDate.set');

    const tableDataMock = jasmine.createSpy('tableData')
      .and.returnValue('table-value');
    tableDataMock.set = jasmine.createSpy('tableData.set');

    // Assign to component
    component.selectedAsOfDate = selectedAsOfDateMock;
    component.tableData = tableDataMock;

    // Stub the internal function
    spyOn(component, 'settingAccessibilityId').and.returnValue('mocked-id');

    // Trigger the lifecycle method
    component.ngOnChanges();

    // Assert
    expect(selectedAsOfDateMock.set).toHaveBeenCalledWith('01/01/2024');
    expect(tableDataMock.set).toHaveBeenCalledWith('mocked-id');
  });
});