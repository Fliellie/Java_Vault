![[Pasted image 20260331203858.png|992]]

![[Pasted image 20260331203924.png|989]]



scene cũng có thể cài kich thước
```java

//scene cũng có thể cài kich thước
Scecne scene=new Scene(root,600,600,Color.LIGHTSKYBLUE);
//tạo chữ
Text text=new Text();
text.setText("ABC");
text.setX(50);
text.setY(50);


//thêm text lên scene
// lấy root của scene
root.getChildren().add(text);
// thay đổi font cho text
text.setFont(Font.font("Verdana",50));// font and size
// đổi màu
text.setFill(Color.LIMEGREEN);
// tạo 1 đường kẻ
Line line=new Line();
line.setStartX(int);
line.setStartY(int);
line.setEnd(int);
line.setEnd(int);
line.setStrokeWidth(5);// độ dày
line.setStroke(Color.RED);// set màu 
line.setOpacity(0.5); // độ trong suốt
line.setRotate(45); //set góc độ
// hình chữ nhật
Rectangle rec =new Rectangle();
rec.setX();
rec.setY();
rec.setWidth();
rec.setHeight();
// thêm chữ nhật vào scene
root.getChildren().add(rec);
// đổi màu
rec.

```