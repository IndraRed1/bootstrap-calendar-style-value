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

    // Simulate model structure with set()
    component.selectedAsOfDate = {
      set: mockSetSelectedDate
    } as any;

    component.tableData = {
      set: mockSetTableData
    } as any;

    // Override the return values of selectedAsOfDate() and tableData()
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