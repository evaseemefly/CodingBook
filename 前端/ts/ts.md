## 1 常用的库

## 5. 类和接口

```js
type File='A'|'B'|'C'|'D'
type Color='Black'|'White'
type Rank=1|2|3|4

class Position{
   constructor(private file:File,private rank:Rank){}
}

class Piece{
   protected position:Position
   constructor(
      private readonly color:Color,
      file:File,
      rank:Rank
   ){
      this.position=new Position(file,rank)
   }
}
```
