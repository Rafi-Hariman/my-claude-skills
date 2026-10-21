---
name: api-usage
description: Global API usage patterns for all services in UII Gateway Bill Angular project. Use when making any HTTP requests (GET, POST, PUT, DELETE) through BillService, BillCoreService, BillPostgraduateService, BillUnitService, or any other service.
tools: Read, Glob, Bash
---

# Global API Usage Patterns

Standardised patterns for all HTTP operations across the entire project. Every service follows the same architecture and methods.

## Service Architecture

All services implement identical interfaces:

| Service | Location | Purpose |
|---------|----------|---------|
| `BillService` | `services/bill.service.ts` | Main bill operations |
| `BillCoreService` | `service-ugw-bill/bill-core.service.ts` | Core bill functions |
| `BillPostgraduateService` | `service-ugw-bill/bill-postgraduate.service.ts` | Postgraduate operations |
| `BillUnitService` | `service-ugw-bill/bill-unit.service.ts` | Unit bill operations |
| `BillSppService` | `service-ugw-bill/bill-spp.service.ts` | SPP operations |
| `BillCaturdharmaService` | `service-ugw-bill/bill-caturdharma.service.ts` | Catur Dharma operations |
| `BillRemedialService` | `service-ugw-bill/bill-remedial.service.ts` | Remedial operations |
| `BillNonAcademicService` | `service-ugw-bill/bill-non-academic.service.ts` | Non-academic operations |
| `BillDiscountService` | `service-ugw-bill/bill-discount.service.ts` | Discount operations |
| `BillDispensationService` | `service-ugw-bill/bill-dispensation.service.ts` | Dispensation operations |

## Universal Service Methods

All services implement these identical methods:

```typescript
// GET Operations
billSearch(type, params?)        // Search with query parameters
countBill(type, params?)         // Get count
detail(type, params?)            // Get single record
listAll(type, params?, pathParams?)  // List all records
listOverview(type, params?)      // List overview data
pagedList(type, page, callback, params?)  // Paginated list

// POST/PUT/DELETE Operations
insert(type, body, pathParams?)  // POST create
update(type, body, pathParams?)  // PUT update
delete(type, body, pathParams?)  // DELETE
deleteWithBody(type, body, pathParams?)  // DELETE with body
```

## Usage Patterns

### Pattern 1: List All Records (Dropdowns, Master Data)

```typescript
this.billSvc.listAll(BillServiceType.BILL_CORE_BILL_TYPE).subscribe(res => {
  const data = _.orderBy(res['data'], ['jenis_tagihan'], ['asc']) ?? [];
  this.selectOptions.jenis_tagihan = [
    { jenis_tagihan: this.translateSvc.instant('label.all'), kd_jenis_tagihan: '' },
    ...data
  ];
});
```

### Pattern 2: Create New Record (POST)

```typescript
this.isLoading = true;
this.disableBtnSubmit = true;

const formData = {
  nama: this.form.get('nama').value,
  keterangan: this.form.get('keterangan').value,
};

this.billSvc.insert(BillServiceType.BILL_DISCOUNT_SUBMISSION, formData)
  .pipe(finalize(() => {
    this.isLoading = false;
    this.disableBtnSubmit = false;
  }))
  .subscribe(() => {
    this.coreSvc.growl(NOTIFICATION_TYPE.SUCCESS, 'Data berhasil disimpan', () => {}, 2000);
    this.router.navigate(['/back-to-list']);
  });
```

### Pattern 3: Update Record (PUT)

```typescript
this.isLoadingSave = true;

this.billSvc.update(BillServiceType.BILL_CORE_BILL_ONE, requestBody)
  .pipe(finalize(() => {
    this.isLoadingSave = false;
  }))
  .subscribe((response) => {
    this.coreSvc.growl(NOTIFICATION_TYPE.SUCCESS, response.message || 'Data berhasil diubah');
    this.onClose.next(true);
    this.activeModal.hide();
  });
```

### Pattern 4: Delete Single Record

```typescript
this.billSvc.delete(BillServiceType.BILL_STUDY_COST_SCHEMAS_DELETE, { uuid: recordUuid })
  .subscribe(() => {
    this.coreSvc.growl(NOTIFICATION_TYPE.SUCCESS, 'Data berhasil dihapus');
    this.loadData();
  });
```

### Pattern 5: Delete Multiple Records (Batch)

```typescript
const deleteObservables = this.deletedItems.map(item =>
  this.billSvc.delete(BillServiceType.BILL_STUDY_COST_SCHEMAS_DELETE, { uuid: item.uuid })
    .pipe(takeUntil(this.destroy$))
);

forkJoin(deleteObservables).subscribe(() => {
  this.coreSvc.growl(NOTIFICATION_TYPE.SUCCESS, 'Semua data berhasil dihapus');
  this.loadData();
});
```

### Pattern 6: Paginated List (Table Data)

```typescript
this.loading = true;
this.page.pageNumber = 1;
this.page.size = 10;

this.billSvc.pagedList(
  BillServiceType.BILL_CORE_BILL,
  this.page,
  (result: any) => {
    this.rows = result['data'] ?? [];
    this.rows.forEach(element => {
      element.status_label = this.paymentStatus.transform(element.kd_status);
      element.tgl_formatted = this.datePipe.transform(element.tgl, 'dd MMMM y HH:mm:ss');
    });
    this.page = result.page;
    this.loading = false;
  },
  { filter: 'active' }
);
```

### Pattern 7: Get Single Record Detail

```typescript
this.billSvc.detail(BillServiceType.BILL_STUDENT_DETAIL, { id: billId })
  .subscribe(detail => {
    this.record = detail['data'];
  });
```

### Pattern 8: Search with Parameters

```typescript
const params = {
  keyword: this.searchKeyword,
  faculty: this.selectedFaculty,
  year: this.selectedYear
};

this.billSvc.billSearch(BillServiceType.BILL_MONITOR_SEARCH_BILL, params)
  .subscribe(res => {
    this.searchResults = res['data'] ?? [];
  });
```

### Pattern 9: With Path Parameters

```typescript
// GET with path param
this.billSvc.listAll(BillServiceType.BILL_STUDENT_DETAIL, {}, `/${billId}`).subscribe();

// POST with path param (query string)
this.billSvc.insert(BillServiceType.BILL_STUDY_COST_SCHEMAS_CREATE, formData, `?uuid=${uuid}`).subscribe();
```

### Pattern 10: Count Records

```typescript
this.billSvc.countBill(BillServiceType.BILL_STUDENT_COUNT, { status: 'unpaid' })
  .subscribe(res => {
    this.unpaidCount = res['data']?.count || 0;
  });
```

## Standard Response Structure

All API responses follow this structure:

```typescript
interface ApiResponse<T = any> {
  message: string;        // User-friendly message
  data: T;               // Response payload
  page?: {               // Pagination metadata (for pagedList)
    current_page: number;
    total: number;
    per_page: number;
    last_page: number;
  };
}
```

## Loading State Management

### Before API Call

```typescript
this.isLoading = true;
this.disableBtnSubmit = true;
this.loadingBarSvc.start();
```

### After API Call (Both Success and Error)

```typescript
.pipe(finalize(() => {
  this.isLoading = false;
  this.disableBtnSubmit = false;
  this.loadingBarSvc.complete();
}))
```

## Notification Types

```typescript
enum NOTIFICATION_TYPE {
  SUCCESS = 'success',
  ERROR = 'error',
  WARNING = 'warning',
  INFO = 'info'
}

// Usage
this.coreSvc.growl(NOTIFICATION_TYPE.SUCCESS, 'Message text', () => {}, 2000);
```

## Subscription Management

### takeUntil Pattern (Recommended)

```typescript
private destroy$ = new Subject<void>();

ngOnInit() {
  this.billSvc.listAll(type, params).pipe(takeUntil(this.destroy$)).subscribe();
}

ngOnDestroy() {
  this.destroy$.next();
  this.destroy$.complete();
}
```

## Quick Reference

| Operation | Method | Example Service Type |
|-----------|--------|---------------------|
| List all | `listAll(type, params, pathParams)` | `BillServiceType.BILL_CORE_BILL_TYPE` |
| Paginated | `pagedList(type, page, callback, params)` | `BillServiceType.BILL_CORE_BILL` |
| Detail | `detail(type, params)` | `BillServiceType.BILL_STUDENT_DETAIL` |
| Search | `billSearch(type, params)` | `BillServiceType.BILL_MONITOR_SEARCH_BILL` |
| Count | `countBill(type, params)` | `BillServiceType.BILL_STUDENT_COUNT` |
| Create | `insert(type, body, pathParams)` | `BillServiceType.BILL_DISCOUNT_SUBMISSION` |
| Update | `update(type, body, pathParams)` | `BillServiceType.BILL_CORE_BILL_ONE` |
| Delete | `delete(type, body, pathParams)` | `BillServiceType.BILL_STUDY_COST_SCHEMAS_DELETE` |
