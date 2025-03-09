---
layout: default
title: Sub-Modules
description: 🥝 KiwiCursor
light_mode: false
---

Functions within sub-modules, you can find these modules below the main KiwiCursor

> [Go to home page](./)

# Table of Contents

* A markdown unordered list which will be replaced with the ToC, excluding the "Contents header" from above
{:toc}

# KiwiDistance

> Internal: No

Used as a replacement to [MaxDistance](https://create.roblox.com/docs/reference/engine/classes/SurfaceGui#MaxDistance)'s weird way of handling distance, you can disable this distance hiding by turning ``KiwiDistance.Settings.Override`` off

## Extra Notes

### Gui:GetAttribute("Kiwi_MaxDistance")

Max distance, set this to your prefered distance, automatically sets this to ``Gui.MaxDistance``, but if you create this attribute it will prefer it

### Gui:GetAttribute("Kiwi_CurrentDistance")

Current distance from GUI, calculated from ``SurfaceGui - (FaceofGuiPosition - CameraPosition).Magnitude`` and ``BillboardGui - (BillboardWorldPosition - CameraPosition).Magnitude``

## Functions

### KiwiDistance.AddSignal()

> Parameters: **``(Gui: Gui, VisibleChanged: Signal, DistanceChanged: Signal)``**
> 
> Returns: None
>
> Notes: **Internal function only**

Used by KiwiCursor for connecting ``.ConnectSurface()`` and ``.ConnectBillboard()``

### KiwiDistance.RemoveSignal()

> Parameters: **``(Gui: Gui)``**
> 
> Returns: None
>
> Notes: **Internal function only**

Used by KiwiCursor for removing ``Kiwi.ConnectSurface()`` and ``Kiwi.ConnectBillboard()`` connections

# KiwiCollisions

> Internal: No

Gui collisions handler for KiwiCursor

## Functions

### KiwiCollisions.GetGuiCorners()

> Parameters: **``(Position: Vector2, Size: Vector2, Rotation: number)``**
> 
> Returns: **``{TopLeft: Vector2, BottomLeft: Vector2, TopRight: Vector2, BottomRight: Vector2}``**

Gets the corners of the specified area, contains code from [@jaipack17](https://jaipack17.github.io/) that has been cleaned up a bit

```lua
local InsetCorners = Kiwi.GetGuiCorners(AbsPos + (Vector2.one * AbsRadius), AbsSize - (Vector2.one * (AbsRadius*2)), Rotation)
local WithinInsetRadius = false
	
for i,v in pairs(InsetCorners) do
  local mag = (v - Position).Magnitude
  if mag < AbsRadius then WithinInsetRadius = true break end
end
```

### KiwiCollisions.GuiIntersecting()

> Parameters: **``(GuiObject: GuiObject, Position: Vector2)``**
> 
> Returns: **``Intersecting: boolean``**
>
> Notes: **Use ``Kiwi.GuiTouching()`` for a list of gui**

If the provided GuiObject is intersecting with the provided position, accounts for UICorner and Rotation

```lua
print(
  GuiObject .. "is intersecting: " .. tostring(
    KiwiDistance.GuiIntersecting( GuiObject, Vector2.new( 200, 300 ) )
  )
)
```

### KiwiCollisions.GuiTouching()

> Parameters: **``(Parent: Instance, Position: Vector2)``**
> 
> Returns: **``{GuiObject}``**
>
> Notes: **Use ``Kiwi.GuiIntersecting()`` for a single object use**

Returns a list of intersecting gui that are descendants of the parent

```lua
local Hits = KiwiCollisions.GuiTouching(Parent, GuiObject)
for index,value in pairs(Hits) do
  if value:IsA("GuiButton") then
      print("Hovering on button", value)
  end
end
```

# KiwiCommand

> Internal: Yes

Handles commands, you can adjust the settings in the file, such as changing the ``KiwiCommand.Settings.Prefix`` or disabling commands

# KiwiUtility

> Internal: Yes

Handles internal scripts, also contains the attribution text sent in console

> ⚠️ Documentation for KiwiUtility is in the file for people wanting to use it



