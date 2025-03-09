---
layout: default
title: Functions
description: 🥝 KiwiCursor
light_mode: false
---

Functions are easy ways to interact with the cursor, does not include [Sub-Modules](./submodules-page.html)

> [Go to home page](./)

# Table Of Contents

* A markdown unordered list which will be replaced with the ToC, excluding the "Contents header" from above
{:toc}

# Kiwi.Changed

> Parameters: None
> 
> Returns: **``{Signal}``**
> 
> Notes **Connect only**

[RBXScriptSignals](https://create.roblox.com/docs/reference/engine/datatypes/RBXScriptSignal) used to detect when a value in KiwiCursor (such as ``Kiwi.Values``) changes

### Example

```lua
Kiwi.Changed.InScreen:Connect(function()
  print(Kiwi.Values.InScreen)
end)
```

### Guide

```lua
-- List
Kiwi.Changed = {
	InScreen = signal.new(),    -- Fired when (Kiwi.Values.InScreen) changes
	Hovering = signal.new(),    -- Fired when (Kiwi.Values.Hover) changes
	Draggable = signal.new(),   -- Fired when (Kiwi.Values.Draggable) changes
	Mouse = signal.new(),       -- Fired when (Kiwi.Values.Position) changes
	
	-- \\ Gui
	SurfaceGui = signal.new(),  -- Fired when (Kiwi.Values.Surface.Gui) changes
	BillboardGui = signal.new(),-- Fired when (Kiwi.Values.Billboard.Gui) changes
}

```

# Kiwi.GuiVisible()

> Parameters: **``(Frame: GuiObject)``**
> 
> Returns: **``(Visible: boolean, GuiParent: Gui)``**

Accounts for ancestor's visibility, doesn't account for blocking gui or offscreen

### Example

```lua
local Visible, GuiParent = Kiwi.GuiVisible(GuiObject)
if Visible then
  GuiObject.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
end
```

# Kiwi.CFrameToSurfaceGui()

> Parameters: **``(SurfaceGui: SurfaceGui, Value: CFrame)``**
> 
> Returns: **``(AbsolutePosition: Vector2)``**
>

Localizes the CFrame to the SurfaceGui as an AbsolutePosition

### Example

```lua
Part:GetPropertyChangedSignal("CFrame"):Connect(function()
  local PartOnSurface = Kiwi.CFrameToSurfaceGui(SurfaceGui, Part.CFrame)
  GuiObject.Position = UDim2.fromOffset(PartOnSurface.X, PartOnSurface.Y)
end)
```

# Kiwi.Vector2ToBillboardGui()

> Parameters: **``(BillboardGui: BillboardGui, Value: Vector2, (Optional) IgnoreCovering: boolean)``**
> 
> Returns: **``(WithinBillboard: boolean, AbsolutePosition: Vector2, Distance: number)``**
>
> Notes: **Does not support ExtentsOffset**

Localizes the AbsolutePosition to the BillboardGui, any objects covering the billboard will result in a 0,0 position

- IgnoreCovering will ignore covering parts

### Example

```lua
local FrameOnBillboard = Kiwi.Vector2ToBillboardGui(BillboardGui, Frame.AbsolutePosition)
Frame.BackgroundColor3 = (FrameOnBillboard and GreenColor) or (RedColor)
```

# Kiwi.HoveringOverButton()

> Parameters: None
> 
> Returns: **``(HoveringOverButton: boolean)``**

Checks if hovering over a button

### Example

```lua
local Hovering = Kiwi.HoveringOverButton()
if Hovering then
  print("hovering")
end
```

# Kiwi.UpdateMouse()

> Parameters: **``( (Optional) QuickUpdate: boolean)``**
> 
> Returns: **``(HoveringOverButton: boolean)``**
>
> Note: **The mouse will only update during mouse input unless ``Kiwi.Settings.AlwaysUpdate`` is true**

Updates the icon, settings, and position of mouse

- QuickUpdate will skip mouse interpolation

### Example

```lua
Kiwi.Cursors.Grab.Enabled = true
Kiwi.UpdateMouse()
```

# Kiwi.ConnectObject()

> Parameters: **``(GuiObject: GuiObject)``**
> 
> Returns: **``{any}``**

Connect any GuiObjects, more consistent with cursor

**Mouse**

- MouseEnter: Signal ``More reliable enter/leave``
  
- MouseLeave: Signal
  
- MouseMoved: Signal ``Returns: (LocalPosition: UDim2.fromOffset)``

**Click**

-	LeftClick: Signal ``Fully clicks the object``
  
-	RightClick: Signal
  
-	MiddleClick: Signal

**Input**

-	LeftDown: Signal ``Non-button support``
  
- LeftUp: Signal
  
- RightDown: Signal
  
- RightUp: Signal
  
- MiddleDown: Signal ``Added support for middle click``
  
- MiddleUp: Signal
  
**Drag**

- Draggable: Boolean ``Set drag state``

- DragStarted: Signal ``Returns: (DragOffset: UDim2.fromOffset)``
  
- DragMoved: Signal ``Returns: (LocalPosition: UDim2.fromOffset, DragOffset: UDim2.fromOffset)``

- DragEnded: Signal

**Functions**

- 	Disconnect: Function ``Remove connections``

### Example

```lua
local Connection = Kiwi.ConnectObject(GuiObject)
Connection.MiddleClick:Connect(function()
  print("Hello World!")
  Connection.Disconnect()
end)
```

# Kiwi.ConnectBillboard()

> Parameters: **``(BillboardGui: BillboardGui)``**
> 
> Returns: **``{any}``**
>
> Note: **Use ``ConnectSurface()`` for SurfaceGui**

Connect any BillboardGui, more consistent with cursor

**Distance**

- VisibleChanged: Signal ``Returns when visibility is changed``
  
- DistanceChanged: Signal ``Distance from gui changed``
  
**Functions**

- GetGuiPosition: Function ``Gets 3D position,  accounts for all offsets besides for ExtentsOffset and SizeOffset``
  
- Disconnect: Function ``Removes connections``

### Example

```lua
local Connection = Kiwi.ConnectBillboard(BillboardGui)
Connection.VisibleChanged:Connect(function(visible)
  print(visible)
end)
```

# Kiwi.ConnectSurface()

> Parameters: **``(SurfaceGui: SurfaceGui)``**
> 
> Returns: **``{any}``**
>
> Note: **Use ``ConnectBillboard()`` for BillboardGui**

Connect any SurfaceGui, more consistent with cursor

**Distance**

- VisibleChanged: Signal ``Returns when visibility is changed``
  
- DistanceChanged: Signal ``Distance from gui changed``
  
**Functions**

- GetGuiPosition: Function ``Gets 3D position from SurfaceGui.Face``
  
- Disconnect: Function ``Removes connections``

### Example

```lua
local Connection = Kiwi.ConnectSurface(SurfaceGui)
Connection.VisibleChanged:Connect(function(visible)
  print(visible)
end)
```
