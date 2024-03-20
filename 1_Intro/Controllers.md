Controller binded to View folder

![alt text](../_/image.png)

![alt text](../_/image-1.png)

```csharp
//PieController.cs
 public ViewResult List() {
    return View(_pieRepository.AllPies);
```

![alt text](../_/image-2.png)

![alt text](../_/image-3.png)