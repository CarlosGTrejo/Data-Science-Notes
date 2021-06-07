# TOC
* [[#Plotting with Glyphs]]
* [[#Markers]]
* [[#Additional Glyphs]]
* [[#Multiple Plots]]
	* [[#Layouts]]
	* [[#Gridplots]]
	* [[#Tabbed Layouts]]
* [[#Data Formats]]
* [[#Hover]]
* [[#Color Mapping]]
* [[#Linking Plots]]
	* [[#Linking Axes]]
	* [[#Linking Selections]]
* [[#Annotations and Guides]]
	* [[#Legends]]
	* [[#Hover Tooltips]]
****

## Plotting with Glyphs
```py
 from bokeh.io import output_file, show
 from bokeh.plotting import figure
 
 plot = figure(plot_width=400, tools='pan,box_zoom')
 plot.circle([1,2,3,4,5], [8,6,5,2,3])  # x and y coords for circle points
 
 output_file('circle.html')  # export plot to html file
 show(plot)  # view plot
 
 
 ```
 ****
 
 ## Markers
 * `p.asterisk()`
 * `p.circle()`
 * `p.circle_cross()`
 * `p.circle_x()`
 * `p.cross()`
 * `p.diamond()`
 * `p.diamond_cross()`
 * `p.inverted_triangle()`
 * `p.square()`
 * `p.square_cross()`
 * `p.square_x()`
 * `p.triangle()`
 * `p.x()`
****

## Additional Glyphs
* `p.annulus()`
* `p.annular_wedge()`
* `p.wedge()`
* `p.rect()`
* `p.quad()`
* `p.vbar()`
* `p.hbar()`
* `p.image()`
* `p.image_rgba()`
* `p.image_url()`
* `p.patch()`
* `p.patches()`
* `p.line()`
* `p.multi_line()`
* `p.circle()`
* `p.oval()`
* `p.ellipse()`
* `p.arc()`
* `p.quadratic()`
* `p.bezier()`
****

## Multiple Plots

### Layouts
```py
# Layouts can be nested to create more sophisticated layouts
from bokeh.layouts import column, row

layout = row(p1, p2, p3)  # All 3 plots will be shown in a row in the order that they were passed

layout = column(p1, p2, p3)  # All plots will be shown in a column in the order that they were passed

layout = row(column(p1, p2), p3)  # The first section of the row will be a column with p1 over p2, and the other section will be p3 (see diagram below)
#  p1  p3
#  p2

show(layout)
```


### Gridplots
```py
from bokeh.layouts import gridplot

layout = gridplot([
	[None, p1],  # Use `None` as a placeholder where you don't want to place a plot, all lists must be same length
	[p2, p3]],
	toolbar_location=None)  # You can also change the toolbar location or pass None to hide toolbar
show(layout)
```
![[gridplots.png]]

### Tabbed Layouts
```py
from bokeh.models.widgets import Panel, Tabs

# Create a Panel with a title for each tab
first = Panel(child=row(p1, p2), title='first')  # You can pass plots, controls, or rows/columns of plots and controls to `child`
second = Panel(child=row(p3), title='second')

tabs = Tabs(tabs=[first, second])

show(tabs)
```
![[tabbed_layouts.png]]
****

## Data Formats
```py
# When plotting data with Bokeh you can use: arrays, lists, dataframes, and ColumnDataSource

from bokeh.models import ColumnDataSource

source = ColumnDataSource(data={
					'x': [1,2,3,4],
					'y': [5,6,7,8]})

# To access the data...
print(source.data)

# You can also cast a dataframe to a CDS (ColumnDataSource)
source = ColumnDataSource(df)

# Which can then be used to plot like so...
p.circle('df_column_name', 'df_column_name', color='df_color_column_name', source=your_ColumnDataSource)
```

### Another Example of ColumnDataSource
```py
from bokeh.plotting import ColumnDataSource

# Create a ColumnDataSource from df: source
source = ColumnDataSource(df)

# Add circle glyphs to the figure p
p.circle('Year', 'Time', color\='color', size\=8, source\=source)

# Specify the name of the output file and show the result
output\_file('sprint.html')

show(p)
```
****

## Selections
```py
p = figure(tools='box_select, lasso_select')
p.circle(x_data, y_data,
	selection_color='red',  # The selected points will be colored red
	nonselection_fill_alpha=0.2  # Points that fall outside the selection will have an alpha value of 0.2
	nonselection_fill_color='grey')  # Points outside the selection will be grey
	
# You can prefix "selection" or "nonselection" to property names like size, color, alpha, etc.
```
****

## Hover
```py
from bokeh.models import HoverTool

hover = HoverTool(tooltips=None, mode='hline')  # You can also use 'vline'

p = figure(tools=[hover, 'crosshair'])

# You can prefix "hover" to property names like size, color, alpha, fill_color, etc. to change the appearance of the points you hover over.
p.circle(x_data, y_data, size=15, hover_color='red')  # Dots that are on the vertical line of the crosshair will be colored red.
```
****

## Color Mapping
```py
# Usefull if you want to color certain data points a different color (kind of like conditional coloring)
from bokeh.models import CategoricalColorMapper

mapper = CategoricalColorMapper(
	factors=['setosa', 'virginica', 'versicolor'],  # The values to map to the colors (this from the 'species' column in the flower sample dataframe)
	palette=['red', 'green', 'blue'])  # The colors to map to each category (in order). See bokeh.palletes for more colors
	
p = figure(x_axis_label='petal_length', y_axis_label='sepal_length')

p.circle('petal_length', 'sepal_length', source=source,
	size=10,
	color={
		'field': 'species',  # The column where the color mapper's "factors" are in
		'transform': mapper  # The mapper we created with `CategoricalColorMapper`
	})
```
****

## Linking Plots

### Linking Axes
```py
# Do this to make sure your plots have the same x range and/or y range
p3.x_range = p2.x_range = p1.x_range
p3.y_range = p2.y_range = p1.y_range
```

### Linking Selections
```py
# It is important that your plots share a common ColumnDataSource (see `Data Formats` in this document for more info) to link selections
p1 = figure(title='petal length vs. sepal length')
p1.circle('petal_length', 'sepal_length',
	color='blue', source=source)  # <= shared data source

p2 = figure(title='petal length vs. sepal width')
p2.circle('petal_length', 'sepal_width',
	color='green', source=source)  # <= shared data source
	
p3 = figure(title='petal length vs. petal width')
p3.circle('pteal_length', 'petal_width',
	color='red', fill_color=None,
	source=source)  # <= shared data source
```
![[linking_selections.png]]
****

## Annotations and Guides

### Legends
```py
p.circle('petal_length', 'sepal_length',
	size=10, source=source,
	color={'field': 'species',
		'transform': mapper},
	legend='species')

p.legend.location = 'top_left'
```
![[legends.png]]

### Hover Tooltips
```py
from bokeh.models import HoverTool

hover = HoverTool(tooltips=[
	('species name', '@species'),  # The first string is the "label", if the second string is prefixed with `@` it will be treated as a variable and be replaced with actual data
	('petal length', '@petal_length'),  # You can combine normal words and "variables" for the second string if you wish to do so.
	('sepal length', '@sepal_length')
])

p = figure(tools=[hover, 'pan', 'wheel_zoom'])
```
![[hover_tooltips.png]]
****

```
1st cd\_esp\_data  

2nd sing\_espl

3rd cd_esp_scripts

4rd launch jupyter

to access data do cd_esp_data
O is ring
G is garmin watch
```