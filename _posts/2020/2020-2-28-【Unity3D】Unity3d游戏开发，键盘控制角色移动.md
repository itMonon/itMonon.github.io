---
layout: post
category: Unity3D-Daily
title: 【Unity3D】Unity3d游戏开发，键盘控制角色移动
tagline: by 恬静的小魔龙
tag: Unity3D
---

<pre class="has">
<code>public class CharController : MonoBehaviour

{

public Transform hero;

public bool cameraSpace = true;

//all of these parameters are used in Update and Move

public float speed = 10f;

public float acceleration = 10f;

public float shiftAcceleration = 1.5f;

public float jumpSpeed = 5f;

public bool gravity = true;

public Vector3 velocity = Vector3.zero;

public Vector3 forceVelocity = Vector3.zero;

public bool inAir = true;

public Vector3 capsuleP1;

public Vector3 capsuleP2;

public float capsuleR;



//public float stepDist = 0.25f;

public float stepsPerSecond = 50;



public bool useLag = false;

public float lagTime = 0.1f;

public float lagTimeLeft;

public float oldTime;



public static readonly int[] searchDirVert = {1,-1, 0, 0, 1, 1,-1,-1};

public static readonly int[] searchDirHor = {0, 0, 1,-1,-1, 1, 1,-1};





public void Update ()

{

//finding hero

if (hero == null) hero = transform;



//taking capsule params

CapsuleCollider capsuleCollider = hero.GetComponent&lt;CapsuleCollider&gt;();

if (capsuleCollider != null)

{

capsuleP1 = capsuleCollider.center + Vector3.up*(capsuleCollider.height/2-capsuleCollider.radius);

capsuleP2 = capsuleCollider.center - Vector3.up*(capsuleCollider.height/2-capsuleCollider.radius);

capsuleR = capsuleCollider.radius;

}



//emulating lag

lagTimeLeft-=Time.deltaTime;

if (useLag &amp;&amp; lagTimeLeft &gt; 0) return;

lagTimeLeft = lagTime;



//determining look direction

Vector3 lookDir;

if (cameraSpace)

{

lookDir = new Vector3(Camera.main.transform.forward.x, Camera.main.transform.forward.y, Camera.main.transform.forward.z);

if (gravity) lookDir.y = 0;

}

else lookDir = hero.transform.forward;

lookDir = lookDir.normalized;

Vector3 strafeDir= Vector3.Cross(Vector3.up, lookDir);

//moving

Vector3 direction = new Vector3(0,0,0);

if (Input.GetKey (KeyCode.W) || Input.GetKey (KeyCode.UpArrow)) {direction += lookDir; }

if (Input.GetKey (KeyCode.S) || Input.GetKey (KeyCode.DownArrow)) {direction -= lookDir; }

if (Input.GetKey (KeyCode.D) || Input.GetKey (KeyCode.RightArrow)) {direction += strafeDir; }

if (Input.GetKey (KeyCode.A) || Input.GetKey (KeyCode.LeftArrow)) {direction -= strafeDir; }

if (Input.GetKey (KeyCode.Space) &amp;&amp; !inAir) {forceVelocity.y = jumpSpeed; }

if (Input.GetKeyDown (KeyCode.LeftShift)) speed *= shiftAcceleration;

if (Input.GetKeyUp (KeyCode.LeftShift)) speed /= shiftAcceleration;



direction *= speed*10;



float deltaTime = Time.deltaTime; if (useLag) deltaTime = Time.realtimeSinceStartup-oldTime;

hero.position = MoveChar(hero.position, direction, deltaTime); //use deltaTime when not debugging

oldTime = Time.realtimeSinceStartup;

}



public Vector3 MoveChar (Vector3 pos, Vector3 direction, float time) //direction is where controller sends char, velocity is it's old velocity

{

//calculating number of iterations

int numSteps = (int)(time * stepsPerSecond) + 1;

float stepTime = time / numSteps;

if (numSteps &gt; stepsPerSecond/10f) numSteps = (int)(stepsPerSecond/10f); //breaking speed if fps is too low



for (int i=0; i&lt;numSteps; i++)

{

Vector3 initialPos = pos; //to prevent stuck

//adjusting velocity

if (direction.sqrMagnitude &gt; 0.01f) //if moving

{

velocity += direction.normalized*acceleration*stepTime;

velocity = Vector3.ClampMagnitude(velocity, speed);

}

else

{

Vector3 velocityModifier = velocity.normalized*acceleration*stepTime;

if (velocity.sqrMagnitude &gt; velocityModifier.sqrMagnitude) velocity -= velocityModifier; //if still moving

else velocity = Vector3.zero;

}



//moving

pos = TryMove(pos, velocity*stepTime);



//applying gravity

if (gravity)

{

forceVelocity += Vector3.down*stepTime*9.8f; //accelerating fall

if (forceVelocity.y*stepTime &gt; capsuleR) forceVelocity.y = capsuleR/stepTime; //limiting velocity

Vector3 fallPos = pos + forceVelocity*stepTime;



if (!Physics.CheckSphere(fallPos+capsuleP2, capsuleR) &amp;&amp; !Physics.CheckSphere(fallPos+capsuleP1, capsuleR) ) //maybe capsuleP1 is not necessary

{ pos = fallPos; inAir = true; } //falling if nothing under feet

else { forceVelocity.y = 0; inAir = false; } //stopping fall if ground detected

}



//checking stuck

if (Physics.CheckSphere(pos+capsuleP1, capsuleR) || Physics.CheckSphere(pos+capsuleP2, capsuleR) || Physics.Linecast(initialPos,pos))

{

Debug.Log("CharController stuck");

pos = initialPos;



if (Physics.CheckSphere(pos+capsuleP1, capsuleR) || Physics.CheckSphere(pos+capsuleP2, capsuleR))

{ Debug.Log("CharController locked"); pos = GetOutofStuck(pos); }

}

//else Debug.Log("notstuck");

}



return pos;

}





public Vector3 TryMove (Vector3 pos, Vector3 moveVector)

{

UnityEngine.Profiling.Profiler.BeginSample("TryMove");

//check if char can go straight

if (!Physics.CheckSphere(pos+moveVector+capsuleP1, capsuleR) &amp;&amp; !Physics.CheckSphere(pos+moveVector+capsuleP2, capsuleR))

{ UnityEngine.Profiling.Profiler.EndSample(); return pos + moveVector; }



//preparing vectors

Vector3 perpHor = Vector3.Cross(moveVector, Vector3.up).normalized;

Vector3 perpVert = Vector3.Cross(moveVector, perpHor).normalized;

float moveDist = moveVector.magnitude;

//if char cannot go straight - finding alternative position

for (float i=0.5f; i&lt;100f; i=i*1.5f+0.5f)

for (int dir=0; dir&lt;8; dir++)

{

Vector3 possibleDir = (moveVector + perpHor*searchDirHor[dir]*i*moveDist*0.1f + perpVert*searchDirVert[dir]*i*moveDist*0.1f).normalized;

possibleDir *= Vector3.Dot(moveVector, possibleDir);

//possibleDir = possibleDir*0.5f + possibleDir.normalized*moveDist * 0.5f;



if (!Physics.CheckSphere(pos+possibleDir+capsuleP1, capsuleR) &amp;&amp; !Physics.CheckSphere(pos+possibleDir+capsuleP2, capsuleR))

{ UnityEngine.Profiling.Profiler.EndSample(); return pos+possibleDir; }

}



//if no alternative position could be found - returning original pos

UnityEngine.Profiling.Profiler.EndSample();

return pos;

}



public Vector3 GetOutofStuck (Vector3 pos)

{

for (float dist = 0.07f; dist&lt;3f; dist*=1.5f)

for (int xi=0; xi&lt;20; xi++)

for (int zi=0; zi&lt;20; zi++)

{

int x=0; int z=0;

if (xi%2 == 0) x=xi*xi; else x=-(xi+1)*(xi+1);

if (zi%2 == 0) z=zi*zi; else z=-(zi+1)*(zi+1);

Vector3 dir = (new Vector3(x,150,z)).normalized;

dir.y -= 0.5f;

dir = dir.normalized * dist;



if (!Physics.CheckSphere(pos+dir+capsuleP1, capsuleR) &amp;&amp; !Physics.CheckSphere(pos+dir+capsuleP2, capsuleR))

return pos+dir;

}

return pos;

}



public void OnDrawGizmos ()

{

//GetOutofStuck (transform.position);

//Voxeland.Visualizer.DrawGizmos();

/*float moveDist = 1;

Vector3 moveVector = new Vector3(1,0,0);

Vector3 perpHor = Vector3.Cross(moveVector, Vector3.up).normalized;

Vector3 perpVert = Vector3.Cross(moveVector, perpHor).normalized;



for (float i=0.5f; i&lt;100f; i=i*1.5f+0.5f)

//for (float i=1; i&lt;100f; i*=2)

for (int dir=0; dir&lt;8; dir++)

{

Vector3 possibleDir = (moveVector + perpHor*searchDirHor[dir]*i*moveDist*0.1f + perpVert*searchDirVert[dir]*i*moveDist*0.1f).normalized;

possibleDir *= Vector3.Dot(moveVector, possibleDir);

//possibleDir = possibleDir*0.5f + possibleDir.normalized*moveDist * 0.5f;

Gizmos.color = Color.red;//new Color(order*10f/255f, 1-order*10f/255f, 0);

Gizmos.DrawLine(transform.position, transform.position+possibleDir);

}

*/

}

}</code></pre>

<p> </p>