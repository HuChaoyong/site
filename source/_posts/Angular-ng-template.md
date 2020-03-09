---
title: Angular ng-template
catalog: true
date: 2020-03-09 22:29:25
subtitle: 
header-img:
tags:
- Angular
---

# ng-template tag.

* ng-template tag is a Structured tag.

## use ngIf with it.

```html
<ng-template [ngIf]="visible">
    <span class="warning">This a Warning!</span>
</ng-template>

<!-- it's same to this. -->
<span class="warning" *ngIf="visible">This a Warning!</span>
```

## use ngFor with it.

```html
<ng-template ngFor let-item [ngForOf]="items" let-idx="index">
    <span class="list-item">{{idx}}: {{item.name}}</span>
</ng-template>

<!-- it's same to this. -->
<span class="list-item" *ngFor="let item of items; index as idx">{{idx}}: {{item.name}}</span>
```

## use model variable.

* some component need ngTemplate param.

```html
<nz-input-group nzSearch [nzAddOnAfter]="suffixIconButton">
    <input type="text" nz-input placeholder="input search text" />
</nz-input-group>

<ng-template #suffixIconButton>
    <button nz-button nzType="primary" nzSearch><i nz-icon type="search"></i></button>
</ng-template>
```