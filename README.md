import { HeroiconComponent } from '@frk/ng-ui-core';
import { PRODUCTS_SERVICE } from '@frk/ng-ui-extra';
import { product99646_IS } from '@frk/shared-data-services/fixtures';
import type { ProductsService } from '@frk/shared-data-services/types';
import { LoggerService, TranslateService } from '@frk/shared-utils';
import { createLoggerMockService } from '@frk/shared-utils/mocks';
import { Spectator, createComponentFactory } from '@ngneat/spectator';
import { MockComponent } from 'ng-mocks';
import { of } from 'rxjs';
import { type Mock, mock } from 'ts-jest-mocker';
import { AttributionTableComponent } from './attribution-table.component';

describe('AttributionTableComponent', () => {
  let spectator: Spectator<AttributionTableComponent>;
  const mockProductsService: Mock<ProductsService> = mock<ProductsService>();
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
    mockProductsService.getProductDetails$.mockReturnValue(of(product99646_IS));
    spectator = createComponent();
  });

  it('should create', () => {
    expect(spectator.component).toBeTruthy();
  });

  it('should format selectedAsOfDate in ngOnChanges', () => {
    // Arrange: Set an initial date string in the format expected by selectedAsOfDate
    const initialDate = new Date('2025-04-01');
    const initialDateString = initialDate.toLocaleDateString('en-US', {
      month: '2-digit',
      day: '2-digit',
      year: 'numeric',
    }).replace(/(\d+)\/(\d+)\/(\d+)/, '$1/$2/$3'); // Format as MM/DD/YYYY (e.g., "04/01/2025")

    // Use the set() method to update the Signal
    spectator.component.selectedAsOfDate.set(initialDateString);

    // Act: Simulate ngOnChanges by triggering change detection
    spectator.detectChanges();

    // Assert: Check if the selectedAsOfDate is formatted correctly
    expect(spectator.component.selectedAsOfDate()).toBe(initialDateString);
  });
});