---
layout: post
title:  "unity 脚本编写记事"
categories: unity
tags: unity
---

* content
{:toc}

## 协程返回

	不好方式：yield return 0;
	正确方式：yield return null;
	
	不好方式：
	while(!IsComplete) {
		yield return new WaitForSeconds(1f);
	}
	正确方式：
	WaitForSeconds delay = new WaitForSeconds(1f);
	while(!IsComplete) {
		yield return delay;
	}
	
## 对象标记

	不好方式：
	gameObject.tag == "TagString";
	正确方式：
	gameObject.CompareTag("TagString");
	
## 装箱操作

	不好方式：
	ArrayList list = new ArrayList();
	正确方式：
	List<T> list = new List<T>();
	
## GC工作量

	不好方式：
	public struct Item
	{
		public string Name;
		public int Age;
		public Vector3 Postion;
	}
	public Item[] ItemArray;
	正确方式：
	public string[] Names;
	public int[] Ages;
	public Vector3[] Postions;
	
## 更新操作

	不好方式：
	private void Update()
	{
		Collider c = GetComponent<Collider>();
	}
	正确方式：
	private Collider m_c;
	private void Start() 
	{
		m_c = gameObject.GetComponent<Collider>();
	}
	
## 

	
	