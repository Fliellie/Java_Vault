### 4. Nếu bạn muốn đặt tên riêng thì sao? (CSS ID và Class)


**Cách 1: Dùng ID (Dành cho vật thể duy nhất)**

- Trong FXML: 
- `<Button fx:id="myUniqueButton" id="special-btn" />`
    
- Trong CSS:
    

```css
#special-btn { 
    -fx-background-color: red; 
}
```

**Cách 2: Dùng Class riêng (Dành cho một nhóm)**

- Trong FXML: `<Button styleClass="my-custom-style" />`
    
- Trong CSS:
    

CSS

```
.my-custom-style {
    -fx-border-color: yellow;
}
```

---