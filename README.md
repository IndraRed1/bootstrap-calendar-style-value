describe('Row Toggling', () => {
  const mockTableData = {
    countries: [{
      regionName: 'Europe',
      ariaOwns: ['child-row-1'],
      regionData: { portfolio: { averageWeight: 5, totalReturn: 10 } }
    }],
    sectors: [{
      sectorName: 'Technology',
      ariaOwns: ['child-row-2'],
      sectorData: { portfolio: { averageWeight: 8, totalReturn: 15 } }
    }]
  };

  beforeEach(() => {
    spectator.component.tableData.set(mockTableData as unknown as FundAttributionGridDataTypes);
    spectator.detectChanges();
  });

  describe('toggleRow()', () => {
    it('should toggle single row expansion', () => {
      const button = spectator.query('button[class*="parent-row"]');
      const row = spectator.query('tr.parent-row');
      
      // Initial state
      expect(row).toHaveAttribute('aria-expanded', 'false');
      
      // First click
      spectator.click(button!);
      spectator.detectChanges();
      expect(row).toHaveAttribute('aria-expanded', 'true');
      
      // Second click
      spectator.click(button!);
      spectator.detectChanges();
      expect(row).toHaveAttribute('aria-expanded', 'false');
    });

    it('should call toggleRow with mouse event', () => {
      const mockEvent = new MouseEvent('click');
      const spy = spyOn(spectator.component, 'toggleRow');
      
      spectator.component.toggleRow(mockEvent);
      expect(spy).toHaveBeenCalledWith(mockEvent);
    });
  });

  describe('toggleAll()', () => {
    it('should toggle all rows when called', () => {
      const spy = spyOn(spectator.component, 'toggleAll');
      spectator.component.toggleAll();
      expect(spy).toHaveBeenCalled();
    });

    it('should expand/collapse all rows in UI', () => {
      const rows = spectator.queryAll('tr.parent-row');
      const toggleButton = spectator.query('button.toggle-all');
      
      // Initial state
      rows.forEach(row => expect(row).toHaveAttribute('aria-expanded', 'false'));
      
      // Expand all
      spectator.click(toggleButton!);
      spectator.detectChanges();
      rows.forEach(row => expect(row).toHaveAttribute('aria-expanded', 'true'));
      
      // Collapse all
      spectator.click(toggleButton!);
      spectator.detectChanges();
      rows.forEach(row => expect(row).toHaveAttribute('aria-expanded', 'false'));
    });
  });
});