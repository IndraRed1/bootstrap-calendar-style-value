# bootstrap-calendar-style-value
bootstrap calendar style value it('should set formatted date and accessibility id when selectedAsOfDate and tableData are defined', () => {
  const formattedDate = '04/24/2025';

  const mockSet = jest.fn();
  const mockSelectedAsOfDate = jest.fn(() => dayjs('2025-04-24'));
  const mockTableData = jest.fn(() => ({ set: mockSet }));

  component.selectedAsOfDate = {
    set: mockSet,
    call: mockSelectedAsOfDate
  } as any;

  component.tableData = {
    set: mockSet,
    call: mockTableData
  } as any;

  jest.spyOn(component, 'selectedAsOfDate', 'get').mockReturnValue(mockSelectedAsOfDate);
  jest.spyOn(component, 'tableData', 'get').mockReturnValue(mockTableData);
  jest.spyOn(component, 'settingAccessibilityId' as any, 'get').mockReturnValue((data: any) => `id-${data}`);

  component.ngOnChanges();

  expect(mockSet).toHaveBeenCalledWith(
    dayjs('2025-04-24').format('MM/DD/YYYY')
  );
  expect(mockSet).toHaveBeenCalledWith('id-[object Object]');
});
