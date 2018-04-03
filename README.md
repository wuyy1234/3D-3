# 3D-3  

## 1、简答并用程序验证

* 游戏对象运动的本质是什么？  
  游戏对象的运动的本质是游戏对象随时间和空间的变化，空间的变化包括transform属性中的position和rotation。  

* 请用三种方法以上方法，实现物体的抛物线运动。（如，修改Transform属性，使用向量Vector3的方法…）  
  
  * 第一种，利用position.transform的坐标直接计算，代码如下：  
   
```  
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class NewBehaviourScript : MonoBehaviour {
	private float speed=5f;
	private float gravity=9.8f;
	// Use this for initialization
	void Start () {
		Debug.Log ("start");
	}
	
	// Update is called once per frame
	void Update () {
		this.transform.position += Vector3.left * Time.time * speed;
		this.transform.position += Vector3.down * Time.time * Time.time /2 * gravity;
		Debug.Log ("time: " + Time.time);
	}
}
```  
  * 第二种，利用对象Rigidbody属性给物体加上重力作用（Use Gravity	If enabled, the object is affected by gravity.）  
  然后在代码框加上水平初速度：  
```    
  this.transform.position += Vector3.left * Time.time * speed;  
```  
  <img src="http://imglf6.nosdn.127.net/img/aHBnT05NNXVUK2g5U29qYVNmeFZYcGozUVQ3OU00bis3VS9yUVFTeFhvU3Z2WWtSME5GWU5nPT0.png?imageView&thumbnail=500x0&quality=96&stripmeta=0"  />  
  
  * 第三种，先声明一个瞬间方向向量vector3，然后将他与position实时相加。  
```      
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class NewBehaviourScript : MonoBehaviour {
	private float speed=0.15f;
	private float gravity=0.98f;
	// Use this for initialization
	void Start () {
		Debug.Log ("start");
	}
	
	// Update is called once per frame
	void Update () {
		Vector3 temp=new Vector3(Time.time * speed , -Time.time * Time.time /2 * gravity,0);
		this.transform.position += temp;
	}
}
```    

*  写一个程序，实现一个完整的太阳系， 其他星球围绕太阳的转速必须不一样，且不在一个法平面上。  
  下面是两张图片，分别是运动开始前和开始后的画面：  
  <img src="http://imglf5.nosdn.127.net/img/aHBnT05NNXVUK2c5bWZXQkdDTU5ScGRVdmhYNTR6bmcybWJ4Q0V1eTJmSjgxMThFbFYxZXJnPT0.png?imageView&thumbnail=500x0&quality=96&stripmeta=0"  />  
  <img src="http://imglf6.nosdn.127.net/img/aHBnT05NNXVUK2c5bWZXQkdDTU5Sb25TcklCbFRwR0Zrc1RWSitWUHMxaUZVRXZRaHNneVhnPT0.png?imageView&thumbnail=500x0&quality=96&stripmeta=0"  />  
  
  下面分别是太阳，地球，月球的程序：  
```    
  public class sun : MonoBehaviour {
	private float speed=0.3f;
	void Update () {
		this.transform.Rotate (Vector3.up*speed, Space.World);//自转
	}
}
```    

```    

  public class earth : MonoBehaviour {
	private float RotateSpeed=1f;
	private Transform origin;  
	private float speed = 20f;  
	float ry, rz;  
	void Start () {
		origin=GameObject.FindWithTag("sun").transform;
	}
	void Update () {
		this.transform.RotateAround(origin.position, Vector3.up ,  speed*Time.deltaTime); 
		this.transform.Rotate (Vector3.up*RotateSpeed, Space.World);

	}
```    

```    
  public class moon : MonoBehaviour {
	public Transform origin;  
	private float speed = 200f;  
	void Start () {
		  origin=GameObject.FindWithTag("earth").transform;
	}
	void Update () {
		  this.transform.RotateAround(origin.position, Vector3.up ,  speed*Time.deltaTime);  
	}
}
```    
  
## 2、编程实践

* 阅读以下游戏脚本
Priests and Devils

Priests and Devils is a puzzle game in which you will help the Priests and Devils to cross the river within the time limit. There are 3 priests and 3 devils at one side of the river. They all want to get to the other side of this river, but there is only one boat and this boat can only carry two persons each time. And there must be one person steering the boat from one side to the other side. In the flash game, you can click on them to move them and click the go button to move the boat to the other direction. If the priests are out numbered by the devils on either side of the river, they get killed and the game is over. You can try it in many > ways. Keep all priests alive! Good luck!

程序需要满足的要求：  
  

* 列出游戏中提及的事物（Objects）  
  游戏角色：3牧师，3魔鬼  
  场景：两个河岸，河，船  
  
  
* 用表格列出玩家动作表（规则表），注意，动作越少越好  
 
| 玩家动作       | 条件    | 
| --------   | -----:   | 
|      开船   |  船在开始岸、船在结束岸、船上有人     |     
|    开始岸的牧师上船     |   	船在开始岸，船有空位，开始岸有牧师    |  
|    开始岸的魔鬼上船     |    船在开始岸，船有空位，开始岸有魔鬼   |   
|    结束岸的牧师上船     |    船在结束岸，船有空位，结束岸有牧师   |   
|    结束岸的魔鬼上船     |    船在结束岸，船有空位，结束岸有魔鬼   |  


* 请将游戏中对象做成预制  
* 在 GenGameObjects 中创建 长方形、正方形、球 及其色彩代表游戏中的对象。  
  
<img src="http://imglf4.nosdn.127.net/img/aHBnT05NNXVUK2p2NFZjWEpWYTkyOXpJWlpaV3F3MmZFZFFWajAxTGljODFnYk5tOG8zWTB3PT0.png?imageView&thumbnail=500x0&quality=96&stripmeta=0"  />  
	
* 使用 C# 集合类型 有效组织对象  
 ```    
 Stack<Character> boatStack;//使用stack来存储船上的物体	
```    

* 整个游戏仅 主摄像机 和 一个 Empty 对象， 其他对象必须代码动态生成！！！ 。 整个游戏不许出现 Find 游戏对象， SendMessage 这类突破程序结构的 通讯耦合 语句。 违背本条准则，不给分  
* 请使用课件架构图编程，不接受非 MVC 结构程序  

下面是程序总体框架，包括 View ,Controller ,Model  ,游戏对象只包括了Camera和Empty的GameObject，其余对象通过程序运行过程中实例化产生。

<img src="http://imglf6.nosdn.127.net/img/aHBnT05NNXVUK2cvN0s1YUpBT1V6WEIwM0Q4eGlkdVVaZU56VWVseWIyK0lxVTFlY3QwcWN3PT0.png?imageView&thumbnail=500x0&quality=96&stripmeta=0"  />  

* 注意细节，例如：船未靠岸，牧师与魔鬼上下船运动中，均不能接受用户事件！  

* 项目整体总结：  
  * 不足之处：  
    * MVC的区分并不是非常清晰，虽然分开了，但有些类似于GUI的显示并没有完全放在View中  
    * 整体视觉做得不好，时间不太够我来细化  
    * 最重要的，框架一开始没有整理好，以至于我思路完全乱了，各个类的函数相互调用和参数传递乱糟糟的，后来有UML重新整理了整体框架逐渐恢复  
    * 实例化对象数组的时候，一开始 忘了给对象数组  new 申请空间，记住要new两次，如下  
    
```    
characters = new Character[6];  
for(int i=0;i<3;i++){  
	characters [i] = new Character ("priest",i);  
	characters [i+3] = new Character ("devil",i);  
}
```    
    * 移动的函数应该放到 void  Update() 里面,如下：  
    
```    
void Update(){  
	boat.transform.position = Vector3.MoveTowards (boat.transform.position, rightCoast, BoatSpeed*Time.deltaTime);  
	if(boat.transform.position==rightCoast){  
		isMoving = false;  
		BoatStatus = 1;  
	}
}
```    
    * 实例化包含有 void Start() void Update() 之类的MonoBehaviour特性的函数时，不能够像下面修改：  
      错误示例：  
```    
public class Boat :MonoBehaviour（){}  
Boat boat=new Boat();  
```    
      正确示例：  
```    
public class Boat :MonoBehaviour（){}  
Boat boat=gameObject.AddComponent<Boat>() as Boat;//实例化  
```    

* 游戏截图：  
<img src="http://imglf5.nosdn.127.net/img/aHBnT05NNXVUK2paaCt1bkVtT1RZV2pBcTlDZVFOZUU4UTkzRGZXKzZxWGFXUC9xbk1IenFBPT0.png?imageView&thumbnail=500x0&quality=96&stripmeta=0"  />  
<img src="http://imglf6.nosdn.127.net/img/aHBnT05NNXVUK2paaCt1bkVtT1RZYlFITjVDaDhxYWpOTnF2VlgyQTNNVWZRUXczWlFjeHRBPT0.png?imageView&thumbnail=500x0&quality=96&stripmeta=0"  />  
<img src="http://imglf6.nosdn.127.net/img/aHBnT05NNXVUK2paaCt1bkVtT1RZYzRHR0FDdjIzYkF2WnhqSFVSaFJmUjFvdFZ0ZDJQeFhBPT0.png?imageView&thumbnail=500x0&quality=96&stripmeta=0"  />  
<img src="http://imglf5.nosdn.127.net/img/aHBnT05NNXVUK2paaCt1bkVtT1RZWm42dWxyMUpLekgwcmlZalhDRW53ZUpmcWkrSCtGRUZnPT0.png?imageView&thumbnail=500x0&quality=96&stripmeta=0"  />  

* UML整体框架（完整代码见文末）：  

<img src="http://imglf6.nosdn.127.net/img/aHBnT05NNXVUK2paaCt1bkVtT1RZVEltek9mcC9DN0RaNGRrTENnbEo5b1gxWERhd1l5MGVBPT0.png?imageView&thumbnail=500x0&quality=96&stripmeta=0"  />  
<img src="http://imglf3.nosdn.127.net/img/aHBnT05NNXVUK2paaCt1bkVtT1RZUWRKa3owU2hMU1JxK0MzZmdMYWFyeGt5OG40ejd5U2JnPT0.png?imageView&thumbnail=500x0&quality=96&stripmeta=0"  />  
<img src="http://imglf4.nosdn.127.net/img/aHBnT05NNXVUK2paaCt1bkVtT1RZV2tvL2ZacHNxWFd6SVMxVlFxN0F0dDdCTHZPM0FGNVB3PT0.png?imageView&thumbnail=500x0&quality=96&stripmeta=0"  />  
<img src="http://imglf6.nosdn.127.net/img/aHBnT05NNXVUK2paaCt1bkVtT1RZZTZkbDZkemZYVXBOUWpwOWUwWEdCd0VacC9BTWY3THd3PT0.png?imageView&thumbnail=500x0&quality=96&stripmeta=0"  />  

* 完整代码：  
Model:  
```    
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Com.Model;

namespace Com.Model{
	public class Character {
		public GameObject characterObject;
		public int status;
		public int tag;//用来标记是第几个牧师或鬼
		public string name;
		public Character(){
		}
		public Character(string type,int i){
			if (type == "devil") {
				characterObject = GameObject.Instantiate (Resources.Load ("Prefabs/Devil", typeof(GameObject)), new Vector3 ((5.8f + i), 2, 0), Quaternion.identity, null) as GameObject;
				name="devil";
				status = 1;
				tag = i;
			} else if (type == "priest") {
				characterObject = GameObject.Instantiate(Resources.Load ("Prefabs/Priest", typeof(GameObject)),new Vector3((5.8f+i),1,0), Quaternion.identity,null) as GameObject ;
				name="priest";
				status = 1;
				tag = i;
			}
		}
	}
	public class Boat :MonoBehaviour  {
		public GameObject boat;
		private int BoatSpeed =20;
		Vector3 rightCoast = new Vector3 (3,0, 0);
		Vector3 leftCoast = new Vector3 (-0.5f, 0, 0);
		private int BoatStatus;//-1  左岸，1  右岸
		private bool isMoving;

		Stack<Character> boatStack;

		void Start(){
			boat=GameObject.Instantiate(Resources.Load ("Prefabs/Boat", typeof(GameObject)),new Vector3(3,0,0), Quaternion.identity,null) as GameObject ;
			BoatStatus=1;
			boatStack = new Stack<Character> ();
			isMoving=false;
		}

		public int GetPassengerNum(){
			return boatStack.Count;
		}
		public void toBoat(string characterType,Character[] characters){//上船
			if(boatStack.Count>=2){
				Debug.Log ("the boat is full");
				return;
			}
			else if (characterType == "priest") {
				for (int i = 0; i < 3; i++) {
					if (characters [i].status == BoatStatus) {//在船的同一侧
						if (boatStack.Count == 0) {
							characters [i].characterObject.transform.position = boat.transform.position + new Vector3 (-1, 1, 0);

						} else {//左边有乘客
							characters [i].characterObject.transform.position = boat.transform.position + new Vector3 (0, 1, 0);
						}
						characters [i].characterObject.transform.parent = boat.transform;
						characters [i].status = 0;//修改状态在船上
						boatStack.Push (characters [i]);
						return;
					}
				}
			}
			else if(characterType == "devil"){
				for (int i = 3; i < 6; i++) {
					if (characters [i].status == BoatStatus) {//在船的同一侧
						if (boatStack.Count == 0) {
							characters [i].characterObject.transform.position = boat.transform.position + new Vector3 (-1, 1, 0);

						} else {//左边有乘客
							characters [i].characterObject.transform.position = boat.transform.position + new Vector3 (0, 1, 0);
						}
						characters [i].characterObject.transform.parent = boat.transform;
						characters [i].status = 0;//修改状态在船上
						boatStack.Push (characters [i]);
						return;
					}
				}	
			}
		}
		public void fromBoat(string characterType,Character[] characters){//下船
			if (boatStack.Count <= 0) {
				Debug.Log ("the boat is empty");
				return;
			} 
			else if (characterType == "priest") {
				for (int i = 0; i < 3; i++) {
					if (characters [i].status == 0) {//把它从船上赶下来
						if (BoatStatus == -1) {//左岸下船
							characters[i].characterObject.transform.position=new Vector3( (-6+i),1,0 );
							characters [i].status = -1;
						} 
						else if (BoatStatus == 1) {
							characters[i].characterObject.transform.position=new Vector3( (5.8f+i),1,0 );
							characters [i].status = 1;
						}
						characters [i].characterObject.transform.parent = null;
						boatStack.Pop ();
						return;
					}
				}
			}
			else if (characterType == "devil") {
				for (int i = 0; i < 3; i++) {
					if (characters [i+3].status == 0) {//把它从船上赶下来
						if (BoatStatus == -1) {//左岸下船
							characters[i+3].characterObject.transform.position=new Vector3( (-6+i),2,0 );
							characters [i+3].status = -1;
						} 
						else if (BoatStatus == 1) {
							characters[i+3].characterObject.transform.position=new Vector3( (5.8f+i),2,0 );
							characters [i+3].status = 1;
						}
						characters [i+3].characterObject.transform.parent = null;
						boatStack.Pop ();
						return;
					}
				}
			}
		}	
		void Update(){
			//Debug.Log ("update");
			if (isMoving) {
				Debug.Log ("isMoving  Update");
				if (BoatStatus == -1 && boatStack.Count!=0) {
					boat.transform.position = Vector3.MoveTowards (boat.transform.position, rightCoast, BoatSpeed*Time.deltaTime);
					//Debug.Log ("moveBoat");
					if(boat.transform.position==rightCoast){
						isMoving = false;
						BoatStatus = 1;
					}
				}
				else if(BoatStatus == 1){
					boat.transform.position = Vector3.MoveTowards (boat.transform.position, leftCoast, BoatSpeed*Time.deltaTime );

					if(boat.transform.position==leftCoast){
						isMoving = false;
						BoatStatus = -1;
					}
				}
			}
		}

		public void MoveBoat(){
			isMoving = true;
		}

		public int checkWin(Character[] characters){//判断输赢 ,-1为输，1为赢
			int leftDevil=0;
			int rightDevil = 0;
			int leftPriest = 0;
			int rightPriest = 0;
			int boatPriest = 0;
			int boatDevil = 0;

			for (int i = 0; i < 6; i++) {
				if (characters [i].name == "devil" && characters [i].status == 1) {//鬼在右边
					rightDevil++;
				} else if (characters [i].name == "devil" && characters [i].status == 0) {//鬼在船上
					boatDevil++;
				} else if (characters [i].name == "devil" && characters [i].status == -1) {//鬼在左边
					leftDevil++;
				} else if (characters [i].name == "priest" && characters [i].status == 1) {//牧师在右边
					rightPriest++;
				} else if (characters [i].name == "priest" && characters [i].status == 0) {//牧师在船上
					boatPriest++;
				}
				else if (characters [i].name == "priest" && characters [i].status == -1) {
					leftPriest ++;
				}
			}
			if (BoatStatus == 1) {
				if (((rightDevil + boatDevil) > (rightPriest + boatPriest)) && (rightPriest + boatPriest) != 0) {
					return -1;
				}
				if (leftDevil > leftPriest && leftPriest != 0) {
					return -1;
				}
			}
			if(BoatStatus == -1) {
				if (rightDevil > rightPriest && rightPriest != 0) {
					return -1;
				}
				if ((leftDevil + boatDevil) > (leftPriest + boatPriest) && (leftPriest + boatPriest) != 0) {
					return - 1;
				}
			}
			if (leftDevil == 3 && leftPriest == 3) {
				return 1;
			}
			return 0;
		}
	}
	public class Coast{//用coast代替shore
		GameObject leftCoast;
		GameObject rightCoast;
		public Coast(){
			leftCoast =GameObject.Instantiate(Resources.Load ("Prefabs/Shore", typeof(GameObject)),new Vector3(8.2f,-0.5f,0), Quaternion.identity,null) as GameObject ;
			rightCoast=GameObject.Instantiate(Resources.Load ("Prefabs/Shore", typeof(GameObject)),new Vector3(-6,-0.5f,0), Quaternion.identity,null) as GameObject ;
		}
	}
}

```    
View:  
```    
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Com.Model;
public class View : MonoBehaviour {
	GUIStyle style=new GUIStyle();
	void Start(){
		style.fontSize = 40;
	}
	void OnGUI(){
		GUI.Label (new Rect (Screen.width / 2-100, Screen.height / 2 - 250, 100, 50), "Priest and Devil",style);
	}
}
```    
Controller:  
```    
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Com.Model;

public class Controller : MonoBehaviour {
	View view;
	GUIStyle buttonStyle;
	GUIStyle style=new GUIStyle();
	private int status;
	Boat boat;
	Coast coast;
	Character[] characters;

	void Start () {
		view = gameObject.AddComponent <View>() as View;
		boat = gameObject.AddComponent<Boat>() as Boat;//实例化
		style.fontSize = 40;
	}

	void Awake(){
		Debug.Log ("Controller Awake");
		coast = new Coast ();
		/*GameObject a = new GameObject ();
		a= GameObject.Instantiate (Resources.Load ("Prefabs/Devil", typeof(GameObject)), new Vector3 ((4f), 2, 0), Quaternion.identity, null) as GameObject;
		Debug.Log (a);
		*/
		characters = new Character[6];
		for(int i=0;i<3;i++){
			characters [i] = new Character ("priest",i);
			characters [i+3] = new Character ("devil",i);
		}
	}
	/*int get_status(){
		return status;
	}
	void Update(){
		
	}*/
	void OnGUI(){
		if (boat.checkWin (characters) == 1) {//赢
			GUI.Label (new Rect (Screen.width / 2, Screen.height / 2 + 50, 100, 50), "win",style);
		}
		if (boat.checkWin (characters) == -1) {//输
			GUI.Label (new Rect (Screen.width / 2, Screen.height / 2 +50, 100, 50), "lose",style);
		}


		if (GUI.Button (new Rect (Screen.width / 2 + 250, Screen.height / 2 - 200, 100, 50), "牧师(圆）上")) {
			boat.toBoat ("priest", characters);
			//priest.MovePriest ("to",boat);
		}
		if (GUI.Button (new Rect (Screen.width/2+150,Screen.height/2-200 ,100, 50), "魔鬼（方）上")) {
			boat.toBoat ("devil", characters);

		}

		if (GUI.Button (new Rect (Screen.width/2-250,Screen.height/2-200 ,100, 50), "牧师(圆）下")) {
			boat.fromBoat ("priest", characters);
			//priest.MovePriest ("from",boat);
		}
		if (GUI.Button (new Rect (Screen.width/2-150,Screen.height/2-200,100, 50), "魔鬼（方）下")) {
			boat.fromBoat("devil", characters);

		}

		if ( GUI.Button (new Rect (Screen.width/2,Screen.height/2-200 ,100, 50), "开船") ) {
			//Debug.Log ("Boat");
			boat.MoveBoat ();
		} 
	}
}

```    
