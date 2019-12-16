+++
title = "Fixes some bugs for ng-grid-reorderable"
date =  2016-03-26T22:00:47+08:00
draft = true
tags = ["ng-gird","angularjs"]
categories = ["javascript"]
+++
[ng-grid-reorderable](https://github.com/angular-ui/ng-grid/tree/2.x/plugins) is a drag-drop plugin for [ng-gird](https://github.com/angular-ui/ng-grid). I found some bugs when I was using this plugin.
### Bug 1: It doesn't work on firefox
The plugin use html5 drag-drop, firefox needs to set data for dataTransfer.
```javascript
//set dataTransfer as it's required by firefox
var onDragStart = function (event) {
    event.originalEvent.dataTransfer.setData("gridId", self.myGrid.gridId);
};
```

### Bug 2: Can drag a row to the other ng-grid
To fix this bug, it needs to compare grid id on drop event.
```javascript  
var prevRow = self.services.DomUtilityService.eventStorage.rowToMove;
if (prevRow.gridId != self.myGrid.gridId) return;
```
### Bug 3: Can't select all texts when at edting status
To fix this bug, it needs to disable drag-drop feature.
```javascript
//It should disable drag/drop feature when editing
rowScope.$on('ngGridEventStartCellEdit', function () {
    targetRow.attr('draggable', 'false');
    targetRow.off('dragstart', onDragStart);
});
rowScope.$on('ngGridEventEndCellEdit', function () {
    targetRow.attr('draggable', 'true');
    targetRow.on('dragstart', onDragStart);
});
```
### Bug 4: Can select some texts and then drag and drop it on the ng-gird
To fix this bug, it needs to remove dragover and drop event for the $viewport when editing
```javascript
rowScope.$on('ngGridEventStartCellEdit', function () {
    self.myGrid.$viewport.off('dragover', self.dragOver).off('drop', self.onRowDrop);
});
rowScope.$on('ngGridEventEndCellEdit', function () {
    self.myGrid.$viewport.on('dragover', self.dragOver).on('drop', self.onRowDrop);
});
```
### The codes in gist
{{< gist dixon629 a77d1507f0e65c0e2908  >}}