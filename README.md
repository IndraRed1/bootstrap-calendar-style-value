describe('Row Toggling', () => {
  const mockTableData = { /* ... */ };
  let rows: Element[];
  let toggleButton: Element;

  beforeEach(() => {
    spectator.component.tableData.set(mockTableData as FundAttributionGridDataTypes);
    spectator.detectChanges();
    rows = spectator.queryAll('tr.parent-row');
    toggleButton = spectator.query('button.toggle-all')!;
  });

  const verifyRowStates = (expectedState: string) => {
    for (const row of rows) {
      expect(row).toHaveAttribute('aria-expanded', expectedState);
    }
  };

  describe('toggleAll()', () => {
    it('should expand/collapse all rows in UI', () => {
      // Initial state
      verifyRowStates('false');

      // Expand all
      spectator.click(toggleButton);
      spectator.detectChanges();
      verifyRowStates('true');

      // Collapse all
      spectator.click(toggleButton);
      spectator.detectChanges();
      verifyRowStates('false');
    });
  });
});