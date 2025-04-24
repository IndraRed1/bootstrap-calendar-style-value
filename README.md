it('should update selectedAsOfDate and tableData on ngOnChanges', () => {
  const selectedAsOfDateMock = {
    get: jasmine.createSpy().and.returnValue('2024-01-01'),
    set: jasmine.createSpy()
  };

  const tableDataMock = {
    get: jasmine.createSpy().and.returnValue('mockTableData'),
    set: jasmine.createSpy()
  };

  // Assign the mocks to the component
  component.selectedAsOfDate = () => selectedAsOfDateMock.get();
  component.selectedAsOfDate.set = selectedAsOfDateMock.set;

  component.tableData = () => tableDataMock.get();
  component.tableData.set = tableDataMock.set;

  // Optional: stub the settingAccessibilityId function if it's doing something custom
  spyOn(component, 'settingAccessibilityId').and.returnValue('accessibleId');

  // Trigger ngOnChanges
  component.ngOnChanges();

  // Assert that the date is formatted and set correctly
  expect(selectedAsOfDateMock.set).toHaveBeenCalledWith('01/01/2024');

  // Assert that tableData is updated with accessibility ID
  expect(tableDataMock.set).toHaveBeenCalledWith('accessibleId');
});