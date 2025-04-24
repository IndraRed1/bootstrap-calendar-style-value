import dayjs from 'dayjs';

// ... existing imports ...

describe('AttributionTableComponent', () => {
    // ... existing setup ...

    describe('ngOnChanges', () => {
        it('should format selectedAsOfDate to MM/DD/YYYY when a valid date is set', () => {
            const testDate = '2024-05-20';
            spectator.component.selectedAsOfDate.set(testDate);
            spectator.component.ngOnChanges();
            expect(spectator.component.selectedAsOfDate()).toBe(dayjs(testDate).format('MM/DD/YYYY'));
        });

        it('should not format selectedAsOfDate when it is null', () => {
            spectator.component.selectedAsOfDate.set(null);
            spectator.component.ngOnChanges();
            expect(spectator.component.selectedAsOfDate()).toBeNull();
        });

        it('should apply settingsAccessibilityId to tableData when present', () => {
            const mockData = { someProp: 'value' } as unknown as FundAttributionGridDataTypes;
            const modifiedData = { ...mockData, accessibilityId: 'modified' };
            
            spyOn(spectator.component, 'settingsAccessibilityId').and.returnValue(modifiedData);
            
            spectator.component.tableData.set(mockData);
            spectator.component.ngOnChanges();
            
            expect(spectator.component.settingsAccessibilityId).toHaveBeenCalledWith(mockData);
            expect(spectator.component.tableData()).toEqual(modifiedData);
        });

        it('should not modify tableData when it is null', () => {
            spectator.component.tableData.set(null);
            spectator.component.ngOnChanges();
            expect(spectator.component.tableData()).toBeNull();
        });
    });
});