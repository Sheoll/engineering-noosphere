
<!DOCTYPE html>
<head>
<title>Убери кубики</title>
<meta charset='utf-8'/>
<meta content='IE=Edge,chrome=1' http-equiv='X-UA-Compatible'>
<meta content='width=device-width, initial-scale=1.0' name='viewport'>
  
<style >
label 
{
  display: inline-block;
  margin-bottom: .5rem;
}

h1
{
margin-left:10px;
}

input[type=button],[type=submit]
{
width:120px;
    background:white; 
    border:2px solid black;
   
    
    text-align:center;

}
input[type=text]
{

  text-align: center;
}
#about,#_NewGame
{
  margin-left: 10px;
 margin-right:40px;
}

#pts
{
margin-right:20px;
}

.field
{
width:960px;
height:500px;
border:1px solid #000;
margin-top:20px;
float:left;
margin-left:10px;
}

#record
{
width:325px;
height:500px;
border:1px solid #000;
margin-top:20px;
float:left;
margin-left:20px;
font-size:32px;
text-align:center;
}

.buttons
{
float:left;
margin-left:10px;
display:inline-break;
}
.info
{
 margin-left: 700px;
}
</style>
<script type="text/javascript">


var score=0; //Очки
var _time = 60; //Время

var Player=0;//Количество безымянных игроков
var Color=["red","green","blue","yellow","magenta"]; //Цвет
var Removed=new Array(); //Удалённые кубики

function Start()//Старт
{

  var StartPause=document.getElementById("start");
  
  if(StartPause.value=="Старт") // Старт
   {
       StartPause.value="Пауза";
       var gameField=document.getElementById('field_id');
       for(var i=0;i<160;i++) //Создать кубики
       {
        var newBlock = document.createElement('button');
        newBlock.id="block"+(i+1);
  newBlock.onclick=function(){Incscore(this.id);};
        newBlock.style.backgroundColor=Color[Math.round(Math.random()*4)];
  newBlock.style.width="60px";
        newBlock.style.height="50px";
        newBlock.style.border="2px solid lightgray";
        newBlock.style.outline="none";
  newBlock.style.float="left";
  gameField.appendChild(newBlock);
       }
       _time=60; //Задать таймер
       TIMER(); // Включить таймер
   }
   else 
   {
       if(StartPause.value=="Продолжить") // Продолжить
       {
          StartPause.value="Пауза";
       }
       else // Пауза
       {
         StartPause.value="Продолжить";
       }
   }
}

function Incscore(id)//Когда нажимаем на кубик
{
     var Color_of_Block=document.getElementById(id);
     var start=document.getElementById("start").value;
     var counter = document.getElementById("timer"); 
      
      
      if(Color_of_Block.style.backgroundColor!="white" && start!="Продолжить" && _time>=0) 
      {
         switch(Color_of_Block.style.backgroundColor) 
   {
    case "magenta":score+=1;break;
    case "yellow":score+=2;break;
    case "green":score+=4;break;
    case "blue":score+=8;break;
    case "red":score+=16;break;
    default:break;
   }
        document.getElementById("pts").value=score;
  
   
        Color_of_Block.style.backgroundColor="white";
  Color_of_Block.style.border="2px solid white";
  
        Removed.push(id);
  
      }
}

function NewGame() //Новая игра
{
       for(var i=0;i<160;i++) // Очистить игровое поле
       {
        document.getElementById("block"+(i+1)).parentNode.removeChild(document.getElementById("block"+(i+1)));  
       }
       
       // Вернуть начальные значения
       _time=60;
       document.getElementById("timer").value="01:00";
       document.getElementById("start").value="Старт";
       document.getElementById("pts").value=score=0;
}

function TIMER()//Таймер 
{
    var StartPause=document.getElementById("start");
    
    function tick() 
    {
        var counter = document.getElementById("timer");
  
        if(_time>0&&StartPause.value=="Пауза") // Обратный отсчёт
  {
  _time--;
        counter.value = (_time/60 < 10 ? "0" : "") + String(parseInt(_time/60))+ ":"+ (_time%60 < 10 ? "0" : "") +
    String(_time%60);
  }
  
        if( _time >= 0 )
  {
            setTimeout(tick, 1000);
        }
  
   
 
  
if(_time==0)// Конец игры
{

  
  document.getElementById("timer").value="00:00";
  var TableResult=document.getElementById("record");

var PlayerName = prompt("Конец игры\n\nРезультат: "+String(parseInt(score))+"\n\nВведите ваше имя:");
if(PlayerName.length!=0)
{
  TableResult.innerHTML+=" <br/> "+PlayerName+": "+score;
  }
  else 
  {
    TableResult.innerHTML+=" <br/> Player"+ ++Player +": "+score;
  }
  _time=-1;
}

  if(_time%6==0 && Removed.length>=2 && StartPause.value=="Пауза") // Добавление новых кубиков
  {
  
    for(var i=0;i<Math.round(Math.random()*2);i++)
    {
      var ind=Math.round(Math.random()*Removed.length);
      var Color_of_Block=document.getElementById(Removed[ind]);
      if(Color_of_Block.style.backgroundColor=="white")
      {
                Color_of_Block.style.backgroundColor=Color[Math.round(Math.random()*4)];
  Color_of_Block.style.border="2px solid lightgray";
                Removed.splice(ind,1);
      
      }
      
    }
    
  }
    }
    
    if(_time>=-1)
    {
       tick();
    }
}

function Rules()//Правила игры
{
  var About=document.getElementById('forma');
  About.action="../Лада/About.html";
}
</script>

</head>
<body>
<h1>Убери кубики</h1>
<br/>

<div class="buttons" >
<form id="forma" method="GET">
<input type="submit" value="Правила игры" id="about" onclick="Rules()"/>
<input type="button" value="Старт" id="start" onclick="Start()"/>
<input type="button" value="Начать заново" id="_NewGame" onclick="NewGame()"/>
</form>
</div>

<div class="info">
<label>Очки</label>
<input id="pts" type="text" value="0" size="5" readonly>
<label>Время</label>
<input type="text" value="01:00" size="5" id="timer" readonly>

</div>
<div class="field" id="field_id"> </div>
<div id="record">Таблица результатов<br/>
</div>

</body>
</html>
