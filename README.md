import { ModelSignal } from '@angular/core';
import dayjs from 'dayjs';

it('should set formatted date and accessibility id when selectedAsOfDate and tableData are defined', () => {
  const mockSetSelectedDate = jest.fn();
  const mockSetTableData = jest.fn();

  // Create mock signals
  const mockSelectedSignal = {
    set: mockSetSelectedDate,
    value: '2025-04-24',
  } as unknown as ModelSignal<string | null>;

  const mockTableDataSignal = {
    set: mockSetTableData,
    value: { someKey: 'value' },
  } as unknown as ModelSignal<any>;

  // Assign to component
  component.selectedAsOfDate = mockSelectedSignal;
  component.tableData = mockTableDataSignal;

  // Stub the method that generates the accessibility ID
  jest.spyOn(component as any, 'settingAccessibilityId').mockReturnValue('mock-accessibility-id');

  // Trigger ngOnChanges
  component.ngOnChanges();

  // Expectations
  expect(mockSetSelectedDate).toHaveBeenCalledWith(dayjs('2025-04-24').format('MM/DD/YYYY'));
  expect(mockSetTableData).toHaveBeenCalledWith('mock-accessibility-id');
});