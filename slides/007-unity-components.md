## 1. Components

<img src="https://user-images.githubusercontent.com/7360266/137622846-6ab6d44e-494e-4aeb-ab4c-411b15dcefc2.png" width="400" height="220">

<img src="https://user-images.githubusercontent.com/7360266/137622851-5ba41a62-a267-48e3-88e5-a8b74ca42814.png" width="400" height="200">

- `GameObjects` in Unity have one or more Components
- Components Give a `GameObject` meaning
- A `GameObject` is only something with a Name, 
a Position and optionally a Parent and/or a Child (or more)
- `GameObjects` **ALWAYS** have a Transform
- (Sometimes, it‘s a `RectTransform`)
- Each Component represents one Behaviour of this `GameObject`
- In other Frameworks, `GameObjects` are called `Entities`

---

## 2. Adding Components

<img src="https://user-images.githubusercontent.com/7360266/137623173-425211b4-f46c-4dee-ad97-1873dd46ab6a.png" width="400" height="250">

<img src="https://user-images.githubusercontent.com/7360266/137623183-0c25a99d-fd89-4364-9ad3-50fcc4a5c2d7.png" width="400" height="250">

- You can add more components using the Add Component Button in the Inspector
- Or by Drag-Dropping a Script from the Project-Window

---

## 3. (re)-moving Components

<img src="https://user-images.githubusercontent.com/7360266/137623263-5cd3ca6f-5d00-4c35-9f41-e15debf4be80.png" width="400" height="250">

- You can Remove and Move components using the `ContextMenu` (by Rightclicking)

---

## 4. Configuring Components

<img src="https://user-images.githubusercontent.com/7360266/137623350-43f8d49f-5d57-4b18-a9aa-1da4a71b344d.png" width="400" height="250">

<img src="https://user-images.githubusercontent.com/7360266/137623355-1aae4a5e-b12a-4004-99ab-bd71cd0ae8ad.png" width="500" height="250">

- You can configure components using the inspector
- You can only change variables that are:
  - Either Public
  - Or have a [SerializeField]-Attribute
- Private variables without attributes can not be seen / edited.

---

## 5. Custom Components

<img src="https://user-images.githubusercontent.com/7360266/137623409-dd6d61d7-13ee-416f-b20c-324a32341776.png" width="400" height="250">

<img src="https://user-images.githubusercontent.com/7360266/137623411-db0bc147-2921-457a-8a21-c34ca52339e8.png" width="400" height="250">


- You can create your own Components by using the Create>C# Script Context-Menu within the Project View.
- The component name has to match the File-Name!!
- The component has to be a public class and inherit from MonoBehaviour

---
