GridController (Cheatsheet)
====

12/10/2015

Cheatsheet of Xceed Grid (DataGridControl), GridController (Abbott) and GridViewModel (Abbott)

View:
----

```xaml
<xcdg:DataGridVirtualizingCollectionViewSource
        x:Key="GridItems"
        .../>

<xcdg:DataGridControl 
        x:Name="dgcName"
        ItemSource="{Binding Source={StaticSaurce GridItems}}"
        ...>
```

View Codebehind:
----

  SetModel(IViewModelBase viewModel) <<< this is the ViewModel (in MVVM sense) of the View.
  {DataContext = viewModel;}

  gridController = new GridController(dgcName); <<< hooking up with the ui control in the View
  gridController.ViewModel = DataContext.XyzViewModel; <<< NOT the ViewModel (in MVVM sense), but the GridViewModel

ViewModel:
----

```csharp


VmConstructor(IxxxView view, ....)

view.SetModel(this);

 // used for the GridController
private GridViewModel xyzViewModel;
public GridViewModel XyzViewModel
{
  get
  {
    return summaryViewModel ?? 
          (summaryViewModel = 
            new GridViewModel {
              SearchCriteria = XyzCriteria,
              QueryItems     = (c) => xyzSvc.GetAllXyz(c as MultiFieldFilterQueryCriteria)....
              QueryItemCount = (c) => xyzSvc.GetAllXyzCount(c as MultiFieldFilterQueryCriteria),
              QueryItemIds   = (c) => xyzSvc.GetAllXyzIds(c as MultiFieldFilterQueryCriteria).ToArray(),
              QueryItemId    = (o) => { var oo = o as Carryover; return oo == null ? 0 : oo.CarryoverId; },
              UpdateRow      = (o, n) => { ((Carryover) n).CarryoverFile = ((Carryover) o).CarryoverFile; return true; },
            });
  }
}
```

***

GridViewModel behind the scene (aka paging)
----

The purpose of GridViewModel is to manage paging for the DataGrid (made by Xceed)

Behind the scene, following calls (in order) is made, tagging along with a SearchCriteria:

1) QueryItemCount is called to get TOTAL number of items for the DataGrid. Basically, it is the number of rows in a given database table

2) QueryItemIds is called to get ALL RowIds (identity column) 

Note: given that both calls above are for TOTAL/ALL, SearchCriteria’s
   .PageNumber = 1
   .PageSize = 75 (depends on how we set it up)
   .RowIds = null
   .MaxRowId = 0

   are not used at all.

3) QueryItems is called to get data rows back. SearchCriteria passed in has

   .PageNumber = 1 <<< always 1 for some reason, so we can ignore it!
   .PageSize = 75  <<< this is already taken into account by the GridController/GridViewModel, so we don’t need it in most cases.
   .RowIds = List`<int>` <<< this list integers are actually we use to fetch the needed data rows from database.
   .MaxRowId = the Max Row Id

Note: we do see problem if # of row ids is a huge number, say 1 million! GridController/GridViewModel need to maintain this huge list of row ids—which can consume a lots of memory.

8byte * 10million = 80million = 80Megabyte <<<< seems acceptable, isn’t it?
