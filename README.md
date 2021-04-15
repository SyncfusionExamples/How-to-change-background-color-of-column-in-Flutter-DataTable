# How to change background color of column in Flutter DataTable

The Syncfusion Flutter DataTable widget retrieves the widget from user end itself for each cell. So, users simply wrap the Text widget inside the Container and set the color for that container.

The following example shows how to set background color for ID and Designation columns.
## STEP 1
Create data source class extends with DataGridSource for mapping data to the SfDataGrid. 
You can provide the color to your widget based on the column name in the buildRow method with the help of datagridRowAdapter.

```xml
class EmployeeDataSource extends DataGridSource {
  EmployeeDataSource({required List<Employee> employees}) {
    _employees = employees;
    updateDataGridRows();
  }

  List<DataGridRow> dataGridRow = [];
  late List<Employee> _employees;

  void updateDataGridRows() {
    dataGridRow = _employees
        .map<DataGridRow>((e) => DataGridRow(cells: [
              DataGridCell<int>(columnName: 'id', value: e.id),
              DataGridCell<String>(columnName: 'name', value: e.name),
              DataGridCell<String>(
                  columnName: 'designation', value: e.designation),
              DataGridCell<int>(columnName: 'salary', value: e.salary),
            ]))
        .toList();
  }

  @override
  List<DataGridRow> get rows => dataGridRow;

  @override
  DataGridRowAdapter buildRow(DataGridRow row) {
    return DataGridRowAdapter(
        cells: row.getCells().map<Widget>((dataGridCell) {
      Color? columnBackgroundColor;
      if (dataGridCell.columnName == 'id') {
        columnBackgroundColor = Colors.tealAccent[100];
      } else if (dataGridCell.columnName == 'designation') {
        columnBackgroundColor = Colors.amberAccent[100];
      } else {
        columnBackgroundColor = Colors.transparent;
      }

      return Container(
        alignment: Alignment.center,
        color: columnBackgroundColor,
        padding: EdgeInsets.symmetric(horizontal: 16.0),
        child: Text(dataGridCell.value.toString()),
      );
    }).toList());
  }
}
```
## STEP 2
Initialize the SfDataGrid with all the required properties

```xml
 @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Syncfusion Flutter DataGrid'),
      ),
      body: SfDataGrid(
        source: employeeDataSource,
        columnWidthMode: ColumnWidthMode.fill,
        columns: <GridColumn>[
          GridTextColumn(
              columnName: 'id',
              label: Container(
                  padding: EdgeInsets.all(16.0),
                  alignment: Alignment.center,
                  child: Text(
                    'ID',
                  ))),
          GridTextColumn(
              columnName: 'name',
              label: Container(
                  padding: EdgeInsets.all(8.0),
                  alignment: Alignment.center,
                  child: Text('Name'))),
          GridTextColumn(
              columnName: 'designation',
              label: Container(
                  padding: EdgeInsets.all(8.0),
                  alignment: Alignment.center,
                  child: Text(
                    'Designation',
                    overflow: TextOverflow.ellipsis,
                  ))),
          GridTextColumn(
              columnName: 'salary',
              label: Container(
                  padding: EdgeInsets.all(8.0),
                  alignment: Alignment.center,
                  child: Text('Salary'))),
        ],
      ),
    );
  }

```