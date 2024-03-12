Controller binded to View folder

![alt text](../_images/image.png)

![alt text](../_images/image-1.png)

```csharp
//PieController.cs
 public ViewResult List() {
    return View(_pieRepository.AllPies);
```

![alt text](../_images/image-2.png)

![alt text](../_images/image-3.png)