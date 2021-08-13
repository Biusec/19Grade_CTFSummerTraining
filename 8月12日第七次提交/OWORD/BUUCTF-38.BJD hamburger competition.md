# BUUCTF-38.BJD hamburger competition

1.这道题比较有意思，是用unity编写的一个小游戏，看样子似乎是需要选取不同的食材垒一个小汉堡儿来得到flag，unity程序是用C#编写的，代码在Assembly-CSharp.dll中，可以用dnSpy这个工具反编译

2.浏览发现了一个和flag有关的方法：

```c#
public static string Md5(string str)
	{
		byte[] bytes = Encoding.UTF8.GetBytes(str);
		byte[] array = MD5.Create().ComputeHash(bytes);
		StringBuilder stringBuilder = new StringBuilder();
		foreach (byte b in array)
		{
			stringBuilder.Append(b.ToString("X2"));
		}
		return stringBuilder.ToString().Substring(0, 20);
	}

public static string Sha1(string str)
	{
		byte[] bytes = Encoding.UTF8.GetBytes(str);
		byte[] array = SHA1.Create().ComputeHash(bytes);
		StringBuilder stringBuilder = new StringBuilder();
		foreach (byte b in array)
		{
			stringBuilder.Append(b.ToString("X2"));
		}
		return stringBuilder.ToString();
	}

public void Spawn()
	{
		FruitSpawner component = GameObject.FindWithTag("GameController").GetComponent<FruitSpawner>();
		if (component)
		{
			if (this.audioSources.Length != 0)
			{
				this.audioSources[Random.Range(0, this.audioSources.Length)].Play();
			}
			component.Spawn(this.toSpawn);
			string name = this.toSpawn.name;
			if (name == "汉堡底" && Init.spawnCount == 0)
			{
				Init.secret += 997;
			}
			else if (name == "鸭屁股")
			{
				Init.secret -= 127;
			}
			else if (name == "胡罗贝")
			{
				Init.secret *= 3;
			}
			else if (name == "臭豆腐")
			{
				Init.secret ^= 18;
			}
			else if (name == "俘虏")
			{
				Init.secret += 29;
			}
			else if (name == "白拆")
			{
				Init.secret -= 47;
			}
			else if (name == "美汁汁")
			{
				Init.secret *= 5;
			}
			else if (name == "柠檬")
			{
				Init.secret ^= 87;
			}
			else if (name == "汉堡顶" && Init.spawnCount == 5)
			{
				Init.secret ^= 127;
				string str = Init.secret.ToString();
				if (ButtonSpawnFruit.Sha1(str) == "DD01903921EA24941C26A48F2CEC24E0BB0E8CC7")
				{
					this.result = "BJDCTF{" + ButtonSpawnFruit.Md5(str) + "}";
					Debug.Log(this.result);
				}
			}
			Init.spawnCount++;
			Debug.Log(Init.secret);
			Debug.Log(Init.spawnCount);
		}
	}
```

看来是需要使得secret经sha1加密后为"DD01903921EA24941C26A48F2CEC24E0BB0E8CC7"，flag就为其md5的前20位，解密该字符串得到secret应为“1001”，md5加密后为“B8C37E33DEFDE51CF91E1E03E51657DA”，则flag为BJDCTF{B8C37E33DEFDE51CF91E}