Arrays of sprites:
spriteImage = ImageList.LoadImage("http://litdev.co.uk/game_images/football.png")
numSprite = 10
For i = 1 To numSprite
  sprite[i] = Shapes.AddImage(spriteImage)
EndFor

Sprite Properties:
gw = 600
gh = 600
GraphicsWindow.Width = gw
GraphicsWindow.Height = gh
 
CreateSprites()
 
'Game Loop
While ("True")
  start = Clock.ElapsedMilliseconds
  
  UpdateSprites()
  
  delay = 20 - (Clock.ElapsedMilliseconds - start)
  If (delay > 0) Then
    Program.Delay(delay)
  EndIf
EndWhile
 
Sub CreateSprites
  spriteImage = ImageList.LoadImage("http://litdev.co.uk/game_images/football.png")
  'Sprite dimensions we use the half width and height
  spriteWidth = ImageList.GetWidthOfImage(spriteImage)/2
  spriteHeight = ImageList.GetHeightOfImage(spriteImage)/2
  
  numSprite = 10
  For i = 1 To numSprite
    spriteData["image"] = Shapes.AddImage(spriteImage)
    spriteData["Xpos"] = spriteWidth + Math.GetRandomNumber(gw-2*spriteWidth)
    spriteData["Ypos"] = spriteHeight + Math.GetRandomNumber(gh-2*spriteHeight)
    spriteData["Xvel"] = Math.GetRandomNumber(11)-6
    spriteData["Yvel"] = Math.GetRandomNumber(11)-6
    sprites[i] = spriteData
  EndFor
EndSub
 
Sub UpdateSprites
  For i = 1 To numSprite
    spriteData = sprites[i] 'get current sprite array
    
    'Reposition sprite center
    spriteData["Xpos"] = spriteData["Xpos"] + spriteData["Xvel"]
    spriteData["Ypos"] = spriteData["Ypos"] + spriteData["Yvel"]
    
    'Bounce on walls
    If (spriteData["Xpos"] < spriteWidth) Then
      spriteData["Xpos"] = spriteWidth
      spriteData["Xvel"] = -spriteData["Xvel"]
    ElseIf (spriteData["Xpos"] > gw-spriteWidth) Then
      spriteData["Xpos"] = gw-spriteWidth
      spriteData["Xvel"] = -spriteData["Xvel"]
    EndIf
    If (spriteData["Ypos"] < spriteHeight) Then
      spriteData["Ypos"] = spriteHeight
      spriteData["Yvel"] = -spriteData["Yvel"]
    ElseIf (spriteData["Ypos"] > gh-spriteHeight) Then
      spriteData["Ypos"] = gh-spriteHeight
      spriteData["Yvel"] = -spriteData["Yvel"]
    EndIf
    
    'Move sprite center
    Shapes.Move(spriteData["image"],spriteData["Xpos"]-spriteWidth,spriteData["Ypos"]-spriteHeight)
    
    sprites[i] = spriteData 'save updated sprite array (it may have been modified)
  EndFor
EndSub

Recycling Sprites:

Missile sprites

gw = 600
gh = 600
GraphicsWindow.Width = gw
GraphicsWindow.Height = gh
GraphicsWindow.MouseDown = OnMouseDown
 
CreateSprites()
 
'Game Loop
While ("True")
  start = Clock.ElapsedMilliseconds
  
  If (mouseDown) Then
    FireMissile()
    mouseDown = "False"
  EndIf
  UpdateSprites()
  
  delay = 20 - (Clock.ElapsedMilliseconds - start)
  If (delay > 0) Then
    Program.Delay(delay)
  EndIf
EndWhile
 
Sub CreateSprites
  spriteImage = ImageList.LoadImage("http://litdev.co.uk/game_images/missile.png")
  'Sprite dimensions we use the half width and height
  spriteWidth = ImageList.GetWidthOfImage(spriteImage)/2
  spriteHeight = ImageList.GetHeightOfImage(spriteImage)/2
  
  numSprite = 50
  For i = 1 To numSprite
    spriteData["image"] = Shapes.AddImage(spriteImage)
    spriteData["Xpos"] = spriteWidth + Math.GetRandomNumber(gw-2*spriteWidth)
    spriteData["Ypos"] = gh-spriteHeight
    spriteData["Xvel"] = 0
    spriteData["Yvel"] = -5
    spriteData["Status"] = 0
    Shapes.HideShape(spriteData["image"])
    sprites[i] = spriteData
  EndFor
EndSub
 
Sub UpdateSprites
  For i = 1 To numSprite
    spriteData = sprites[i] 'get current sprite array
    
    If (spriteData["Status"] = 1) Then
      'Reposition sprite center
      spriteData["Xpos"] = spriteData["Xpos"] + spriteData["Xvel"]
      spriteData["Ypos"] = spriteData["Ypos"] + spriteData["Yvel"]
      
      'Move sprite center
      Shapes.Move(spriteData["image"],spriteData["Xpos"]-spriteWidth,spriteData["Ypos"]-spriteHeight)
      
      'Sprite finished with
      If (spriteData["Ypos"] < -spriteHeight) Then
        spriteData["Status"] = 0
        Shapes.HideShape(spriteData["image"])
      EndIf
      
      sprites[i] = spriteData 'save updated sprite array (it may have been modified)
    EndIf
  EndFor
EndSub
 
Sub FireMissile
  For i = 1 To numSprite
    spriteData = sprites[i] 'get current sprite array
    If (spriteData["Status"] = 0) Then
      spriteData["Status"] = 1
      Shapes.ShowShape(spriteData["image"])
      spriteData["Xpos"] = GraphicsWindow.MouseX
      spriteData["Ypos"] = gh-spriteHeight
      
      sprites[i] = spriteData 'save updated sprite array (it may have been modified)
      i = numSprite 'End loop
    EndIf
  EndFor
EndSub
 
Sub OnMouseDown
  mouseDown = "True"
EndSub

Delete Sprites:
gw = 600
gh = 600
GraphicsWindow.Width = gw
GraphicsWindow.Height = gh
GraphicsWindow.MouseDown = OnMouseDown
 
CreateSprites()
 
'Game Loop
While ("True")
  start = Clock.ElapsedMilliseconds
  
  If (mouseDown) Then
    FireMissile()
    mouseDown = "False"
  EndIf
  UpdateSprites()
  
  delay = 20 - (Clock.ElapsedMilliseconds - start)
  If (delay > 0) Then
    Program.Delay(delay)
  EndIf
EndWhile
 
Sub CreateSprites
  spriteImage = ImageList.LoadImage("http://litdev.co.uk/game_images/missile.png")
  'Sprite dimensions we use the half width and height
  spriteWidth = ImageList.GetWidthOfImage(spriteImage)/2
  spriteHeight = ImageList.GetHeightOfImage(spriteImage)/2
  
  sprites = ""
  nextSprite = 1
EndSub
Sub UpdateSprites
  spriteIndices = Array.GetAllIndices(sprites)
  For i = 1 To Array.GetItemCount(spriteIndices)
    index = spriteIndices[i] 'current sprite index
    spriteData = sprites[index] 'get current sprite array
    
    'Reposition sprite center
    spriteData["Xpos"] = spriteData["Xpos"] + spriteData["Xvel"]
    spriteData["Ypos"] = spriteData["Ypos"] + spriteData["Yvel"]
    
    'Move sprite center
    Shapes.Move(spriteData["image"],spriteData["Xpos"]-spriteWidth,spriteData["Ypos"]-spriteHeight)
    
    'Sprite finished with
    If (spriteData["Ypos"] < -spriteHeight) Then
      Shapes.Remove(spriteData["image"])
      Sprites[index] = ""
    Else        
      sprites[index] = spriteData 'save updated sprite array (it may have been modified)
    EndIf
  EndFor
EndSub

Sub FireMissile
  spriteData["image"] = Shapes.AddImage(spriteImage)
  spriteData["Xpos"] = GraphicsWindow.MouseX
  spriteData["Ypos"] = gh-spriteHeight
  spriteData["Xvel"] = 0
  spriteData["Yvel"] = -5
  sprites[nextSprite] = spriteData
  nextSprite = nextSprite+1
EndSub
 
Sub OnMouseDown
  mouseDown = "True"
EndSub

Conclusion:

