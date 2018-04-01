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
|   船的左方下船      |    	船靠岸且船左方有人   |   
|     船的右方下船    |  船靠岸且船右方有人     |   
|    开始岸的牧师上船     |   	船在开始岸，船有空位，开始岸有牧师    |  
|    开始岸的魔鬼上船     |    船在开始岸，船有空位，开始岸有魔鬼   |   
|    结束岸的牧师上船     |    船在结束岸，船有空位，结束岸有牧师   |   
|    结束岸的魔鬼上船     |    船在结束岸，船有空位，结束岸有魔鬼   |  


* 请将游戏中对象做成预制  
* 在 GenGameObjects 中创建 长方形、正方形、球 及其色彩代表游戏中的对象。  
  
<img src="http://imglf4.nosdn.127.net/img/aHBnT05NNXVUK2p2NFZjWEpWYTkyOXpJWlpaV3F3MmZFZFFWajAxTGljODFnYk5tOG8zWTB3PT0.png?imageView&thumbnail=500x0&quality=96&stripmeta=0"  />  
	
* 使用 C# 集合类型 有效组织对象  
* 整个游戏仅 主摄像机 和 一个 Empty 对象， 其他对象必须代码动态生成！！！ 。 整个游戏不许出现 Find 游戏对象， SendMessage 这类突破程序结构的 通讯耦合 语句。 违背本条准则，不给分  
* 请使用课件架构图编程，不接受非 MVC 结构程序  
* 注意细节，例如：船未靠岸，牧师与魔鬼上下船运动中，均不能接受用户事件！  


