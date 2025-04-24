import { ModelSignal } from '@angular/core'; // or wherever it's defined in your project
import { Spectator, createComponentFactory } from '@ngneat/spectator/jest';
import { AttributionTableComponent } from './attribution-table.component';
import dayjs from 'dayjs';

describe('AttributionTableComponent', () => {
  let spectator: Spectator<AttributionTableComponent>;
  let component: AttributionTableComponent;

  const createComponent = createComponentFactory({
    component: AttributionTableComponent,
  });

  beforeEach(() => {
    spectator = createComponent();
    component = spectator.component;
  });

  it('should set formatted date and accessibility id when selectedAsOfDate and tableData are defined', () => {
    const mockSetSelectedDate = jest.fn();
    const mockSetTableData = jest.fn();

    // Fully mocked ModelSignal
    const mockSelectedAsOfDate: Partial<ModelSignal<string | null>> = {
      set: mockSetSelectedDate
    };

    const mockTableData: Partial<ModelSignal<any>> = {
      set: mockSetTableData
    };

    // Assign mocks to the component
    component.selectedAsOfDate = mockSelectedAsOfDate as ModelSignal<string | null>;
    component.tableData = mockTableData as ModelSignal<any>;

    // Mock return values for .value access
    jest.spyOn(component, 'selectedAsOfDate', 'get').mockReturnValue(() => dayjs('2025-04-24'));
    jest.spyOn(component, 'tableData', 'get').mockReturnValue(() => ({ set: mockSetTableData }));

    jest
      .spyOn(component as any, 'settingAccessibilityId')
      .mockReturnValue('mock-accessibility-id');

    component.ngOnChanges();

    expect(mockSetSelectedDate).toHaveBeenCalledWith('04/24/2025');
    expect(mockSetTableData).toHaveBeenCalledWith('mock-accessibility-id');
  });
});