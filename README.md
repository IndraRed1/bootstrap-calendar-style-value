import { ModelSignal } from '@angular/core';

const mockSetSelectedDate = jest.fn();
const mockSetTableData = jest.fn();

const mockSelectedSignal: Partial<ModelSignal<string | null>> = {
  set: mockSetSelectedDate,
  value: '2025-04-24',
};

const mockTableDataSignal: Partial<ModelSignal<any>> = {
  set: mockSetTableData,
  value: { someKey: 'someValue' },
};

component.selectedAsOfDate = mockSelectedSignal as ModelSignal<string | null>;
component.tableData = mockTableDataSignal as ModelSignal<any>;

// Mock the accessibility id method
jest.spyOn(component as any, 'settingAccessibilityId').mockReturnValue('mock-accessibility-id');

component.ngOnChanges();

expect(mockSetSelectedDate).toHaveBeenCalledWith('04/24/2025');
expect(mockSetTableData).toHaveBeenCalledWith('mock-accessibility-id');