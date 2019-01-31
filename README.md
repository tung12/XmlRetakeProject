#Basic Template
##Default HTML

Include those elements

* **ng-template** element: contain filter element for page
* **div class="pxxx-mobile-action"** element: apply filter for mobile
* **pxxx-mobile-show-filter** element: filter icon for mobile
* **div class="m-portlet m-portlet--mobile** element: contain apply filter for desktop , table , chart
* **detail** element: detail component or detail html


```html
<ng-template ></ng-template>

<div class="pxxx-mobile-action" ></div>

<pxxx-mobile-show-filter></pxxx-mobile-show-filter>

<div class="m-portlet m-portlet--mobile"></div>

<!-- Detail -->
<div class="m-portlet m-portlet--mobile" ></div>
```

### *Ng-Template* element
```html
<ng-template #filterTemplate>
  <div class="row align-items-center">
    <div class="col-xs-12 col-sm-6 col-md-4 col-lg-3  m--margin-bottom-10">
      Content element in here
       <label class="col-form-label">
       Element title as Search by, Status, ...
       </label>
       <div class="m-form__control">
       Element as Select, Input ...
       </div>
    </div>
  </div>
</ng-template>
```
```j
#filterTemplate : Id define for ng-template  - use in mobile and desktop layout
```
```j
class="row align-items-center" : cover all elements filter
```
```j
class="col-xs-12 col-sm-6 col-md-4 col-lg-3  m--margin-bottom-10" : Element for search
```

**Example**
- ***One element***
```html
<ng-template #filterTemplate>
  <div class="row align-items-center">
    <!-- Content element in here -->
    <div class="col-xs-12 col-sm-6 col-md-4 col-lg-3  m--margin-bottom-10">  

      <label class="col-form-label">
      <!-- Element title as Search by, Status, ...-->
      {{"PATIENT.MANAGE_PATIENT.search_by" | translate}}
        </label>

      <div class="m-form__control">

      <!-- Element as Select, Input ...-->
        <select class="form-control" [(ngModel)]="filterType">
          <option [value]="type.value" *ngFor="let type of filterTypeArray">
            {{type.name | translate}}
          </option>
        </select>

      </div>
    </div>

    
  </div>
</ng-template>
```
- ***Many element***
```html
<ng-template #filterTemplate>
    <div class="row align-items-center">
      <!-- Content element in here -->
      <div class="col-xs-12 col-sm-6 col-md-4 col-lg-3  m--margin-bottom-10"></div>
      <!-- Content element in here -->
      <div class="col-xs-12 col-sm-6 col-md-4 col-lg-3  m--margin-bottom-10"></div>
       <!-- Content element in here -->
      <div class="col-xs-12 col-sm-6 col-md-4 col-lg-3  m--margin-bottom-10"></div>
       <!-- Content element in here -->
      <div class="col-xs-12 col-sm-6 col-md-4 col-lg-3  m--margin-bottom-10"></div>  
    </div>
  </ng-template>
```


### Div class="pxxx-mobile-action" element:
```html
<div class="pxxx-mobile-action" *ngIf="">
  <div #filterChild class="pxxx-mobile-button pxxx-mobile-filter">
    <ng-container *ngTemplateOutlet="filterTemplate"></ng-container>
  </div>
</div>
```

> **ng-If** condition to show mobile layout of filter
> **#filterChild**  Id define for mobile layout
> ***ngTemplateOutlet** apply filter container to mobile layout base **id** container 


### pxxx-mobile-show-filter element:
```html
<pxxx-mobile-show-filter 
        [input]="local.filter_mobile" 
        (showFilter)="open_filter()" 
        *ngIf="!local.show.patient_detail && !local.show.medical_exam">
</pxxx-mobile-show-filter>
```
> **ng-If** condition to show mobile layout of icon filter
> **showFilter**  function show filter in mobile layout
> ***input** flat for mobile layout  

### class="m-portlet m-portlet--mobile" element:
   ```html
<div class="m-portlet m-portlet--mobile" *ngIf="local.show.patient">
    <div class="m-portlet__head">
      <div class="m-portlet__head-caption">
        <div class="m-portlet__head-title">
          <h3 class="m-portlet__head-text">
            Title page
          </h3>
        </div>
      </div>
    </div>
    <div class="m-portlet__body">
      <!-- apply filter container to desktop layout -->
      <div class="m-form m-form--label-align-right m--margin-bottom-30" *ngIf="!local.is_mobile.matches">
        <ng-container *ngTemplateOutlet="filterTemplate"></ng-container>
      </div>
  
      <!-- Loading element -->
      <div class="row" *ngIf="local.is_loading.patient">
        <div class="col-md-12 col-sm-12 col-xs-12">
          <pxxx-loading [type]="1"></pxxx-loading>
        </div>
      </div>
  
      <div class="row" [hidden]="local.is_loading.patient">
       <!-- Chart --> 
        <div class="col-md-12 col-sm-12">
          <fusioncharts *ngIf="chartData.dataSource.data.length > 0" [width]="chartData._with" [height]="chartData._height"
            dataFormat='json' [type]="chartData._type" [dataSource]="chartData.dataSource">
          </fusioncharts>
        </div>
      <!-- Table --> 
      <div class="m-datatable m-datatable--default m-datatable--brand m-datatable--loaded" [hidden]="local.is_loading.patient">
        <pxxx-table [data]="pxxxTablePatientModel" (done)="onLoadList();" #pxxxTablePatient *ngIf="pxxxTablePatientModel.columns?.length > 0"
          (callback)="retriveCallBack($event)"></pxxx-table>
      </div>
      </div>
    </div>
  </div>
   ```

### Detail element:
```html
<ng-container *ngIf="">
  <!--Detail element in here -->
</ng-container>
```
**Example**
```html
<ng-container *ngIf="local.show.doctor_detail">
  <!--Detail element in here -->
  <app-doctor-detail [dataObj]="detail" [hiddenHtml]="2" (goback)="go_back()"></app-doctor-detail>
</ng-container>
```



## .TS file
```ts
export class ..... implements OnInit, AfterViewInit {

   // add field filter_mobile, is_mobile to local var
  local = {  
    filter_mobile: [],
    is_mobile: null
  };

  // filter mobile
  @ViewChild('filterChild')
  filter_child: MobileFilterComponent;
  filter_mobile: any;

  //add MediaMatcher and function change is_mobile
  constructor(
    private _mediaMatcher: MediaMatcher,
  ) {
    this.local.is_mobile = this._mediaMatcher.matchMedia('(max-width: 768px)');
  }

  ngOnInit() {
   
  }
  ngAfterViewInit() {
    this.get_filter_mobile();
  }

  // add event listener when resize
  @HostListener('window:resize', ['$event'])
  onResize(event) {
    this.local.is_mobile = this._mediaMatcher.matchMedia('(max-width: 768px)');
  }

  //build label search for mobile layout
  get_filter_mobile() {
    
    this.local.filter_mobile = [
      {
        label: 'Từ khóa',
        value:.....
      },
      {
        label: 'Tìm theo',
        value: ....
      },
     .......
    ];
  }


  //open filer div in mobile layout
  open_filter() {
    if (PxxxHelpers.IsDefined(this.filter_child)) {
      this.filter_child.open();
    }
  }

  //add function get_filter_mobile 
  getList() {
    this.get_filter_mobile();
  }

  
}
```
