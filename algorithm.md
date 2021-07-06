### 一、最大公约数和最小公倍数

> 最大公约数，是用递归来求，最小公倍数是两个数相乘并在除以最大公约数

```c++
#include <iostream>
#include <algorithm>
#include <cstdio>
using namespace std;
int gcd (int a, int b)
{
	if (b == 0) return a;
	else
	{
		gcd (b, a % b);
	}
}
int main ()
{   
    int a = 0, b = 0, c = 0;
	cin >> a >> b;
	c = gcd(a,b); //最大公约数 
	int d = 0;
	d = (a * b) / c;//最小公倍数 
	cout << c  << " " << d << endl;
	return 0;
}
//如果要是三个三个的找最小公倍数，那么就两个两个找，先找第一个和第二个，在把第一个和第二个的最小公倍数和最后一个数找最小公倍数。
```

### 二、日期和时间的处理

```c++
关于时间的处理
小时 = x / 3600;
分钟 = x % 3600 / 60;
秒 = x % 60; 
//其他的类似
```

### 三、进制的转化

> 在进制转换的时候，要知道一件事情，关于10-15用A-F表示的时候，要知道10，11，12 ~ 15，是单独的数据，出现类似FF的时候时，应该把他挨个计算，不能当作1515 % 10来计算，只能，把15单独列出来，让15 * n^x，来计算。

```c++
//p进制转化为10进制的方法
//p是代表进制的数字表述
int y = 0, product = 1;
while (x != 0)
{
    y = y + (x % 10) * product;
    x = x / 10;
    product = product * P;
}
//x % 10 表示每次获得，x的最后一位（个位），x / 10,表示把最后一位去掉，让十位变成个位。

//10进制变成 Q进制(除基取余法，正着输入，倒着输出)
int z[40], num = 0;
do
{
    z[num++] = y % Q;
    y = y / Q;
    
}while (y != 0);

```

> 设计10进制到16进制转化的经典题目

[进制转换]:luogu.com.cn/problem/P1143

```c++
#include <iostream>
#include <cstdio>
#include <map>
#include <algorithm>
#include <set>
#include <vector>
#include <cmath>
#include <string>
using namespace std;
int ito(char a) //字母转数字(主要是针对10进制以上的)
{
	if (a == 'A') return 10;
	if (a == 'B') return 11;
	if (a == 'C') return 12;
	if (a == 'D') return 13;
	if (a == 'E') return 14;
	if (a == 'F') return 15;
	return (a - '0');
}
char oti(int a)
{
	if (a == 10) return 'A';
	if (a == 11) return 'B';
	if (a == 12) return 'C';
	if (a == 13) return 'D';
	if (a == 14) return 'E';
	if (a == 15) return 'F';
	return char(a + '0');
}
int main()
{
	int n, m,temp = 0;
	string s,res = "";
	cin >> n;
	getchar();
	cin >> s;
	cin >> m;
	for (int i = 0; i < s.size(); i++)
	{
		 temp = temp  + ito(s[i]) * pow(n, s.size() - i - 1);
		
	}
	int z[40] = { 0 }, count = 0;
	do
	{
		res = oti(temp % m) + res;
		temp /= m;
	} while (temp != 0);
	cout << res;
	return 0;
}
```

> 关于10进制转换成比10进制小的在int范围内不考虑范围的容器(10进制转8进制来说)

```c++
#include <iostream>
#include <cstdio>
#include <map>
#include <algorithm>
#include <set>
#include <vector>
#include <cmath>
#include <string>
#include <stack>
using namespace std;
stack <int> q;
int n;
int main()
{
	cin >> n;
	cout << 0;
	while (n)
	{
		if (n < 8)
		{
			q.push(n);
			break;
		}
		else {
			q.push(n % 8);
			n /= 8;
		}
	}
	while (!q.empty())
	{
		cout << q.top();
		q.pop();
	}
	return 0;
}
```

> 同进制的加法(用到高精度加法概念)

```c++
#include <iostream>
#include <cstdio>
#include <map>
#include <string>
#include <queue> 
#include <cstring>
#include <algorithm>
#include<cmath>
using namespace std;
int n = 0;
vector<int> add(vector<int>& a, vector<int>& b)
{
	if (a.size() < b.size()) return add(b, a);
	vector<int> c;
	int t = 0;
	for (int i = 0; i < a.size(); i++)
	{
		t += a[i];
		if (i < b.size()) t += b[i];
		c.push_back(t % n);
		t /= n;
	}
	if (t) c.push_back(t);
	return c;
}
int main()
{
	vector<int> A, B;
	string a, b;
	cin >> n;
	getchar();
	cin >> a;
	for (int i = a.size() - 1; i >= 0; i--) {
		if (a[i] >= '0' && a[i] <= '9')
		{
			A.push_back(a[i] - '0');
		}
		else if(a[i] >= 'A' && a[i] <= 'Z') {
			A.push_back(a[i] - 'A' + 10);
		}
	}
	getchar();
	cin >> b;
	for (int i = b.size() - 1; i >= 0; i--) {
		if (b[i] >= '0' && b[i] <= '9')
		{
			B.push_back(b[i] - '0');
		}
		else if (b[i] >= 'A' && b[i] <= 'Z') {
			B.push_back(b[i] - 'A' + 10);
		}
	}
	auto c = add(A, B);
	for (int i = c.size() - 1; i >= 0; i--)
	{
		if (c[i] >= 10) {
			cout << char(c[i] + 'A' - 10);
		}
		else cout << c[i];
	}
	return 0;
}
```

> 十六进制转换成二级制，二级制转换成十六机制
>
> 十六进制转成2进制，二进制转成8进制
>
> 这样进制转换的方法

```markdown
十六进制转二进制，四位一体法
A6 - > 二进制
（A = 10 = 8 + 2, 6 = 4 + 2）
(8 4 2 1   8 4 2 1) ->
(1 0 1 0   0 1 1 0)
故A6的二进制为1010 0110

1010 0110 -> 八进制，（三位为一体，不过补三位，缺几个0，补几个0，往最开头补0,三位一体法）
0 1 0   1 0 0   1 1 0（这样分），在利用421，来计算八进制
0*4 + 1 * 2 + 0*1 = 2
1 * 4 + 0*2 + 0*1 = 4
1 * 4 + 1*2 + 0*1 = 6
所以八进制为246
```

具体实例

[1、试题 基础练习 十六进制转八进制]:http://lx.lanqiao.cn/problem.page?gpid=T51

```c
#include <cstring>
#include <string>
#include <iostream>
#include <algorithm>
using namespace std;

int main() 
{
	string s;
	int n = 0;
	cin >> n;
	getchar();
	for (int i = 0; i < n; i++)
	{
		getline(cin, s);
		string t = "";
		for (int j = 0; j < s.size(); j++)
		{
			switch (s[j])
			{
			     case '0': t += "0000"; break;
				 case '1': t += "0001"; break;
				 case '2':t += "0010";  break;
				 case'3':t += "0011";  break;
				 case '4':t += "0100"; break;
				 case '5':t += "0101"; break;
				 case'6':t += "0110"; break;
				 case'7':t += "0111"; break;
				 case '8':t += "1000"; break;
				 case '9':t += "1001"; break;
				 case 'A':t += "1010"; break;
				 case 'B':t += "1011"; break;
				 case 'C':t += "1100"; break;
				 case 'D':t += "1101"; break;
				 case 'E':t += "1110"; break;
				 case 'F':t += "1111"; break;
			}
		}
		int len = t.size();
		//三位一体补0
		if (len % 3 == 1) {
			t = "00" + t;
		}
		else if (len % 3 == 2) {
			t = "0" + t;
		}
		int flag = 0; //排除先导0
		for (int k = 2; k < t.size(); k += 3)
		{
			int sum = 0; //八进制的数值
			sum = (t[k - 2] - '0') * 4 + (t[k - 1] - '0') * 2 + (t[k] - '0') * 1;
			if (sum != 0) flag = 1;
			if (flag == 1) {
				cout << sum;
			}
		}
		cout << endl;
	}
	return 0;
}
```





### 四、素数(又称质数)

```c++
bool isprime(int a)
{
    int b = sqrt(a);
    if (a <= 1) return false;
    for (int i = 2; i <= b; i++)
    {
        if (a %  i == 0) return false;
    }
    return true;
}
```

### 五、质因子的分解

```c++
//首先的是个素数，才是质因子
#include <iostream>
#include <algorithm>
#include <cstdio>
using namespace std;
const int maxn = 100010;
bool isprime(int a)
{
	int b = sqrt(a);
	if (b <= 1) return false;
	for (int i = 2; i <= b;i++)
	{
		if (a % i == 0)
		{
			return false;
		}
	}
	return true;
}
//素数表
int prime[maxn] = {0}, pnum = 0; //存素数的表，个个数
void Find_prime() //求素数表
{
	for (int i = 1; i < maxn; i++)
	{
		if (isprime(i) == true)
		{
			prime[pnum++] = i; //从而有了素数表 
		}
	}
} 

struct factor
{
	int x ;//x为质因子 
	int cnt;//cnt为质因子个数 
} fac[10];
 
int main ()
{   
   Find_prime();
   int n = 0; //n为要分解的数，把它分解为若干质因子
   int num = 0; //num为不同的质因子的个数
   cin >> n;//不过需要进行特判，1的质因子是1 
   if (n == 1) cout << "1 = 1" << endl;
   else
   {
   	    printf("%d = ",n);
   	    int sqr = (int)sqrt(n * 1.0); //对该数进行一半的划分 
   	    //枚举根号以内的质因子
   	    for (int i = 0; i < pnum && prime[i] <= sqr; i++)
   	    {
   	    	if (n % prime[i] == 0)
   	    	{
   	    		fac[num].x = prime[i]; //记录该因子
				fac[num].cnt = 0;
				while (n % prime[i] == 0)
				{
					fac[num].cnt ++;
					n /= prime[i];
				}
				num++; //不同质因子个数加一 
			}
			if (n == 1) break; //及时推出循环，节省时间 
		}
		if (n != 1) //无法被根号n以内的质因子除尽 
	    {
	   	    fac[num].x = n;
			fac[num++].cnt = 1; 
	    }
	    //按照格式输出
		for (int i = 0; i < num; i++)
		{
			if (i > 0) printf("*");
			printf("%d",fac[i].x );
			if (fac[i].cnt > 1)
			{
				printf("^%d",fac[i].cnt );
			}
		} 
   }
   return 0; 
}
```

> 求某个阶乘的质因子分解(故名思意：将一个正整数n写成一个或多个质数乘积的形式)
>
> 我的错误点在于循环出，while（），而我却经常用if（）。

```c++
#include <iostream>
#include <cstdio>
#include <map>
#include <algorithm>
#include <set>
#include <vector>
#include <cmath>
#include <cstring>
#include <stack>
using namespace std;
int bus[100010];
bool isprime(int a)
{
	if (a <= 1) return false;
	int q = sqrt(a);
	for (int i = 2; i <= q; i++)
	{
		if (a % i == 0) return false;
	}
	return true;
}
//各个质因子出现的次方之和为阶乘之和
int main()
{
	int n = 0;
	cin >> n;
	for (int i = 2; i <= i; i++) //判断阶乘的每位数字的质因子，就是该阶乘的质因子
	{
		int j = i; //j存变量i，目的是，利用j去看i的质因子是谁(i是从2到n)
		for (int k = 1; k <= n; k++)
		{
			while ((j % k == 0) && isprime(k) && j > 1) //首先k为j的因子，k必须是质数，某数小于等于1，会让其破化质数性质
			{
				bus[k]++;//存储该质数的因子出现了几次
				j /= k;//目的是能让循环出去，并且判断改整数j在k情况下，有几个质数k.
			}
		}
	}
	for (int i = 0; i <= n; i++)
	{
		if (bus[i]) cout << i << " " << bus[i] << endl;
	}
	return 0;
}
```

> 不用设数组方法：边用边输出，不会有超时问题

```c++
#include <iostream>
#include <cstdio>
#include <map>
#include <algorithm>
#include <set>
#include <vector>
#include <cmath>
#include <cstring>
#include <stack>
using namespace std;
typedef long long ll;

int main()
{
	ll n = 0;
	scanf_s("%lld", &n);
	if (n == 1) { cout << "1=1"; return 0; }
	printf("%lld=", n);
	int flag = 0;
	for (int i = 2; i <= n / i; i++)
	{
		int count = 0;
		while (n % i == 0)
		{
			n /= i;
			count++;
		}
		if (count > 0)
		{
			if (flag == 0)
			{
				flag = 1;
				cout << i;
			}
			else {
				cout << "*" << i;
			}
			if (count > 1) cout << "^" << count;
		}
	}
	if (n > 1)
	{
		if (flag == 1) printf("*%lld", n);
		else printf("%lld", n);
	}
	return 0;
}
```



### 六、分数的表示与化简

如果没有那么复杂，就是让求分数的话，可以这样求

```c++
#include <cstdio>
#include <iostream>
#include <cmath>
using namespace std; 
long long gcd(long long a, long long b) //最大公约数 
{
    a = abs(a);
	b = abs(b);
	if(b == 0) return a;
	else{
		return gcd(b, a % b);
	}
}
int main()
{
	int T = 0;
	cin >> T;
	
	for (int i = 0; i < T; i++)
	{
		int a = 0, b = 0, n = 0;
		cin >> a >> b >> n;
		long double maxn = -1000000;
		long long down = 0, up = 0;
		for (int j = 0; j <= n; j++)
		{
			double f = (a * j + b) * (1.0) / pow(2,j);
			if(f > maxn)
			{
				maxn = f;
				down = pow(2,j);
				up = (a * j + b);
			} 
		}
		long long temp = gcd(up, down);
		down /= temp;
		up /= temp;
		if(down == 1) cout << up << endl; //分子等于1，输出分母
		else {
			cout << up << "/" << down << endl; 
		} 
	} 
	return 0;
}
```



> 强调一点，由于分数的乘法除法可能使分子或分母超过int型表示的范围，因此一般情况下，分子分母应当使用long long型来储存。（建议全部改成long long 型数据，要不然会过不了全部测试点）

> 分数的输出：
>
> 1、输出分数前要对其进行化简
>
> 2、如果分母为1，则直接输出分子
>
> 3、如果分子绝对值大于分母绝对值，假分数。整数部分为r.pu/r.down, 分子部分为abs(r.up) % r.down, 分母部分为r.down
>
> 4、以上均不满足是，说明分数r,是真分数，按原样输出即可。

> 化简的三项规定
>
> 1、分母位负数
>
> 2、分子up为0，分母down为1
>
> 3、约分：求出分子分母绝对值最大公因数d,后令分子分母都除以d;

```c++
//分数的表示与化简 
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#include <cmath>
using namespace std;
struct Fraction
{
	long long up;   //分子 
	long long down; //分母 
}; 

long long gcd (long long a, long long b)  //最大公因数(约数) 
{
	if (b == 0) return a;
	else
	{
		return gcd (b, a % b);
	}
}

Fraction reduction(Fraction result) //分数的化简 
{
	if (result.down < 0) //分母小于0,就让分子分母都变成相反数 
	{
		result.down = - result.down ;
		result.up = - result.up ;
	} 
	if (result.up == 0) //如果分子是0，则必须让分母等于1
	{
		result.down = 1;
	} else {            //分子不是0,进行约分 
		int d = gcd(abs(result.up ),abs(result.down )); //分子分母的最大工约数 
        result.down /= d;
		result.up /= d; 
	}
	return result;
}
//分数的四则运算
Fraction add (Fraction f1, Fraction f2) //分数的和 
{
	Fraction result;
	result.up = f1.up * f2.down + f2.up * f1.down ;
	result.down = f1.down * f2.down ;
	return reduction(result); //化简后的值 
} 

Fraction sub (Fraction f1, Fraction f2) //分数的减
{
	Fraction result;
	result.up = f1.up * f2.down - f2.up * f1.down ;
	result.down = f1.down * f2.down ;
	return reduction(result);
} 

Fraction mul (Fraction f1, Fraction f2) //分数的乘法
{
	Fraction result;
	result.down = f1.down * f2.down ;
	result.up = f1.up * f2.up ;
	return reduction(result);
} 

Fraction div (Fraction f1, Fraction f2) //分数的除法 
{
	Fraction result;
	result.up = f1.up * f2.down ;
	result.down = f1.down * f2.up ;
	return reduction(result);
}
void showResult (Fraction r)  //输出分数r
{
	r = reduction (r);  //1、对分数进行化简
	if (r.down == 1) printf("%lld",r.up ); //2、分母为1，直接输出分子
	else if (abs(r.up) > r.down ) {    //假分数 
		printf("%lld %lld/%lld",r.up/r.down , abs(r.up) % r.down ,r.down );
	} else {  //真分数 
		printf("%lld/%lld",r.up ,r.down );
	}
} 
int main ()
{
    Fraction f1, f2;
    scanf("%lld/%lld %lld/%lld",&f1.up ,&f1.down ,&f2.up ,&f2.down );
    showResult(f1);
    cout << " + ";
    showResult(f2);
    cout << " = ";
    showResult(add(f1,f2));
    cout << endl;
    showResult(f1);
    cout << " - ";
    showResult(f2);
    cout << " = ";
    showResult(sub(f1,f2));
    cout << endl;
    showResult(f1);
    cout << " * ";
    showResult(f2);
    cout << " = ";
    showResult(mul(f1,f2));
    cout << endl;
    showResult(f1);
    cout << " / ";
    showResult(f2);
    cout << " = ";
    if(f2.up != 0) 
    {
    	showResult(div(f1,f2));
	} else {
		printf("Inf");
    }
    cout << endl;
	return 0;
} 

```

### 七、全排列

```c++
//递归法（）
#include <iostream>
#include <cstdio>
#include <map>
#include <algorithm>
#include <set>
#include <vector>
#include <cmath>
#include<stack>
#include<queue>
#include <cstring>
#include <string>
using namespace std;
int p[100]; //存放 当前排列 
int n = 0;
int hashtable[100] = {false}; //判段某数字在当前位是否出现过
 
void dfs(int index) //index 处理当前排列 
{
	if(index == n + 1) //递归边界，且还要知道做什么
	{
		for(int i = 1; i <= n; i++) //找到递归边界后，知道了存放排列值满员了，输出即可
		{
			if(i == 1) cout << p[i];
			else cout << " " << p[i];
		} 
		cout << endl;
		return ;//开始往回返 
	}
	for(int i = 1; i <= n; i++) //寻找1 ~ n开头的排列式子
	{
		if(hashtable[i] == false) //表明该数字还没存入p中
		{
			hashtable[i] = true; 
			p[index] = i;
			dfs(index + 1); //处理index + 1 号排列 
			hashtable[i] = false; //处理完p[index]位x的子问题，还原状态。 
		} 
		
	} 
}
int main()
{
	
	cin >> n;
	dfs(1);
    return 0;
}
```

```c++
//函数法
#include <iostream>
#include <cstdio>
#include <map>
#include <algorithm>
#include <set>
#include <vector>
#include <cmath>
#include<stack>
#include<queue>
#include <cstring>
#include <string>
using namespace std;
int a[100];
int main()
{
	int n = 0;
	cin >> n;
	for(int i = 0; i < n; i++)
	{
		a[i] = i + 1;
	}
	do
	{
		for(int i = 0; i < n; i++)
		{
			cout << a[i] << " ";
		}
		cout << endl;
	}while(next_permutation(a, a + n));
    return 0;
}
```

### 八、排序

#### 1、冒泡排序

```c
/*冒泡排序法：
首先要理解需要进行几趟比较，每趟比较中需要比较多少次
冒泡排序法是（以从小到大排序为例）在第一趟中将第一个大的数与下面的数依次比较知到将最大的数沉入底部其他小的数得到上升，
因为最大的沉入到了底部，所以在趟数减一的同时，每趟的比较次数也减一
第二趟第三趟……也是如此方法，直到全部按从小到大排完为止 
例如，有n个数，需要比较n-1趟，每一趟比较n-1-j趟（j表示这为这是第几趟的序号）过了一趟就少比较一次
且在调换的时候是用每一趟中比较的趟数调换的*/ 
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <string>
using namespace std;
int n = 0, ans = 0;
int a[10010] = { 0 };

void sort1(int a[])
{
	for (int i = 0; i < n - 1; i++)
	{
		for (int j = 0; j < n - 1 - i; j++)
		{
			if (a[j] > a[j + 1]) { swap(a[j], a[j + 1]); }
		}
	}
}
int main()
{
	cin >> n;
	for (int i = 0; i < n; i++)
	{
		cin >> a[i];
	}
	sort1(a);
	for (int i = 0; i < n; i++)
	{
		if (i == 0) cout << a[i];
		else cout << " " << a[i];
	}
	return 0;
}

```

#### 2、选则排序法

```c
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <string>
using namespace std;
int n = 0, ans = 0;
int a[10010] = { 0 };

void sort1(int a[])
{
	for (int i = 0; i < n - 1; i++)
	{
		int temp = i;
		for (int j = i + 1; j < n; j++)
		{
			if (a[temp] > a[j])
			{
				swap(a[temp], a[j]);
				ans++;
			}
		}
	}
}
int main()
{
	cin >> n;
	for (int i = 0; i < n; i++)
	{
		cin >> a[i];
	}
	sort1(a);
	for (int i = 0; i < n; i++)
	{
		if (i == 0) cout << a[i];
		else cout << " " << a[i];
	}
	return 0;
}

```

#### 3、快速排序

```c
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <string>
using namespace std;
int n = 0, ans = 0;
int a[100010] = { 0 };

void sort1(int a[],int left, int right)
{
	if (left >= right) return;
	int i = left - 1, j = right + 1, mid = a[(left + right) / 2];
	while (i < j)
	{
		do i++; while (a[i] < mid);
		do j--; while (a[j] > mid);
		if (i < j)
		{
			swap(a[i], a[j]);
		}
	}
	sort1(a, left, i - 1);
	sort1(a, j + 1, right);
}
int main()
{
	cin >> n;
	for (int i = 0; i < n; i++)
	{
		cin >> a[i];
	}
	sort1(a,0,n -1);
	for (int i = 0; i < n; i++)
	{
		if (i == 0) cout << a[i];
		else cout << " " << a[i];
	}
	return 0;
}

```

#### 4、归并排序

```c
#include <iostream>
using namespace std;
const int maxn = 100;
int a[maxn];
//对数组A的[L1,R1],[L2, R2]区间合并为有序区间
void merge(int a[], int L1, int R1, int L2, int R2)
{
    int i = L1, j = L2;
    int temp[maxn], index = 0;//temp为临时存放合并的数组，index为其下标
    while (i <= R1 && j <= R2)
    {
        if (a[i] <= a[j])
        {
            temp[index++] = a[i++];
        }
        else
        {
            temp[index++] = a[j++];
        }
    }
    while (i <= R1)//将[L1, R1]剩余的元素加入序列temp中
        temp[index++] = a[i++];
    while (j <= R2)//将[L2,R2]剩余的元素加入序列temp中
        temp[index++] = a[j++];
    for (int i = 0; i < index; i++)
    {
        a[L1 + i] = temp[i];将合并后的序列赋值回数组A,
    }
}
void mergeSort(int a[], int left, int right)
{
    if (left < right) //归并排序
    {
        int mid = (left + right) / 2;//取[left,right]的中间值
        mergeSort(a, left, mid);      //递归，将左子区间归并排序
        mergeSort(a, mid + 1, right); //递归，右子区间归并排序
        merge(a, left, mid, mid + 1, right);//将左子区间和右子区间和并
    }
}
int main()
{
    int n = 0;
    cin >> n;
    for (int i = 0; i < n; i++)
    {
        cin >> a[i];
    }
    mergeSort(a, 0, n - 1);
    for (int i = 0; i < n; i++)
    {
        if (i != n - 1)
        {
            cout << a[i] << " ";
        }
        else
        {
            cout << a[i];
        }
    }
    return 0;
}

```

#### 5、堆排序

```c
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <string>
using namespace std;
int n, m;
int heap[100010];
void downadjust(int low, int high)
{
	int i = low, j = i * 2;
	while (j <= high)
	{
		if (j + 1 <= high && heap[j + 1] < heap[j])
		{
			j++;
		}
		if (heap[j] < heap[i])
		{
			swap(heap[i], heap[j]);
			i = j; 
			j = i * 2;
		}
		else {
			break;
		}
	}
}
int main()
{
	cin >> n >> m;
	for (int i = 1; i <= n; i++)
	{
		cin >> heap[i];
	}
	//建堆
	for (int i = n / 2; i; i--)
	{
		downadjust(i, n);
	}
	while (m--)
	{
		cout << heap[1] << " ";
		heap[1] = heap[n--];
		downadjust(1, n);
	}
	return 0;
}

```

#### 6、插入排序

```c
int A[maxn],n; //n为元素个数，数组下标为1~n
void insertsort()
{
    for (int i = 2; i <= n; i++) //进行n-1趟排序
    {
        int temp = A[i],j = i; //temp临时存放A[i],j从i开始往前枚举
        while(j > 1 && temp < A[j - 1])//只要temp小于前一个元素A[j - 1]
        {
            A[j] = A[j - 1];//把A[j - 1] 后移动一位至A[j - 1]
            j--;
        } 
        A[j] = temp;//插入位置为j
    }
}
```

#### 7、桶排序

```c
// 桶排序法：设定一个数组，该数组能装下你输入所有的数据，所以，你输入的数据，是被当做下标处理的
//首先定义你一方位足够大的桶，就是数组
//然后输入数据，让其当作下标并把a[下标]的值加一，在用if语句，加上continue来排除相同的下标，这样，每一份a[下标]的值，都是1，
//再用sum统计每一份a[下标]的个数，就是sum++
//然后用for循环，把下标的范围当成条件，进行输出下标以此方法得到了查重并将其不同的下标值输出
//输出的下标是从小到大，或从到大小的，就完成了排序的功能 
#include <stdio.h>
int main()
{
    int a[1001]={0},N,i,x,sum=0;
    scanf("%d",&N);
	for (i=0;i<N;i++){
		scanf("%d",&x);
		if(a[x]){
			continue;//如果a[i]这个已经加一变成了一，就表示下标的那个值已经存在了，
			        //然后就不对此a[x]赋第二次值，让其变成2，3，或者更多，以达到去重效果 
		}
		a[x]++;
		sum++;//总数量加一 
	}
	printf("%d\n",sum);
	for(i=0;i<1000;i++){
		if(a[i]){//如果a[i]的值已经得到加一，就可以把下标的值输出 
			printf("%d ",i); 
		}
	} 
	printf("\n");
	return 0;
}

```

#### 8、cmp排序，重名次情况

> 成绩相同的学生享有并列的排名，排名并列时，按账号的字母序升序输出==问题在于享有并列的排名==

```c
bool cmp(node a, node b)
{
	if(a.score != b.score)
	{
		return a.score > b.score;
	} else {
		return a.name < b.name;
	}
}//这是cmp的排序
当输入好后，对数组sort,后，可以依次按要求输出名词和分数，但是会有享有相同名次的情况，解决方式
    设定一个数组，b[n],该值是从0 ~ n - 1的，完全可以表示名次，但是有同名次，解决方式为：
b[0] = 1;
	for(int i = 1; i < n; i++)
	{
		if(N[i].score == N[i - 1].score )
		{
			b[i] = b[ i - 1];
		}
		else {
			b[i] = i + 1;
		}
	} 
```



### 九、高精度四则运算

```c++
//高精度加法字符串类型 + 判断是否为回文数
#include <string>
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

string add(string a, string b) //字符串形式加和
{
	string c;
	int i = a.size() - 1;
	int bas = 0, adv = 0; //当前值，进位值 
	while (i >= 0 )
	{
		bas = (a[i] - '0') + (b[i] - '0'); //当前位所得值
		c += (adv + bas) % 10 + '0'; //当前位最终值
		adv = (adv + bas) / 10; //进位值
		--i; 
	} 
	if (adv) c += (adv + '0') ;//若进位值还有值，则做最后的加和
	reverse(c.begin(),c.end()) ;
	return c;
}
bool ispalindromic(string c)
{
	int len = c.size() ;
	bool flag = true;
	for (int i = 0; i < len / 2; i++)
	{
		if (c[i] != c[len - 1 - i]) flag = false;
	}
	return flag;
}

int main ()
{
	string A, B,C;
	cin >> A;
	if (ispalindromic(A))
	{
		cout << A << " is a palindromic number." << endl; //开始即为回文数
		return 0; 
	}
	int cnt = 10;
	bool flag = false;
	while (cnt--) //仅限十步
	{
		B = A;
		reverse(B.begin(),B.end()) ;
		C = add(A, B);
		cout << A << " + " << B << " = " << C << endl;
		if (ispalindromic(C)) //如果c是回文数，则继续操作
		{
			cout << C << " is a palindromic number." << endl;
			flag = true;
			return 0;
		} 
		A = C;
	} 
	if (flag == false) //十步之内未找到回文数
	{
		cout << "Not found in 10 iterations." << endl;
	} 
	return 0;
} 
```

```c++
#include <string>
#include <iostream>
#include <vector>
using namespace std;

vector<int> add(vector<int> &A, vector<int>&B) //高精度加法 
{
	if (A.size() < B.size()) return add(B, A); //保证是长的数据加短的数据
	vector<int>C;
	int t = 0; //进位的做法
	for (int i = 0; i < A.size(); i++)
	{
		t += A[i];
		if(i < B.size())
		{
			t += B[i];
		}
		C.push_back(t % 10); //取个位，进上去值被加到t里面去了
		t /= 10; 
	} 
	if (t) C.push_back(t); //如果还有最后一个进上去的位数没有被加上，通过这条语句来加上
	return C; 
}

int main ()
{
    vector<int>A , B;
	string a, b;
	cin >> a >> b;
	for (int i = a.size() - 1; i >= 0; i--) 
	{
		A.push_back(a[i] - '0'); //倒着存放 
	}
	for (int i = b.size() - 1; i >= 0; i--)
	{
		B.push_back(b[i] - '0'); //倒着存放 
	} 
	auto C = add(A, B);
	for (int i = C.size() - 1; i >= 0; i--) //倒着输出
	{
		cout << C[i];
	} 
	return 0;
} 

```

```c++
//高精度减法
#include <iostream>
#include <string>
#include <vector>
using namespace std;
vector <int> sub(vector<int>&A, vector<int>&B)
{
	if (A.size() < B.size()) return sub(B, A);
	vector<int> C;
	int t = 0; //借位
	for (int i = 0; i < A.size(); i++)
	{
		t = A[i] - t;
		if (i < B.size()) 
		{
			t -= B[i];
			
		}
		C.push_back((t + 10) % 10); //按照借位的方法存入（t负数的话，借一加十，正数的话，取余留个位） 
		//借位处理
		if(t < 0) t = 1; //通过下一次循环，还回去那个1 （t = A[i] - t）
		else t = 0; 
	} 
	while (C.size() > 1 && C.back() == 0) C.pop_back(); //去除002这种情况，只输出2
	return C; 
}

int main ()
{
    string a, b;
    cin >> a >> b;
    vector<int> A, B;
    for (int i = a.size() - 1; i >= 0; i--)
    {
    	A.push_back(a[i] - '0');
	}
	for (int j = b.size() - 1; j >= 0; j--) 
	{
		B.push_back(b[j] - '0');
	}
	auto C = sub(A, B);
	for (int i = C.size() - 1; i >= 0; i--)
	{
		cout << C[i];
	}
	return 0;
} 
```

```c++
//高精度乘以低精度 
#include <iostream>
#include <string>
#include <vector>
using namespace std;
vector <int> mul (vector<int>&A, int b)
{
	vector<int>C;
	int t = 0;
	for (int i = 0; i < A.size() || t; i++) //  ||t的原因是最后如果t存有数据，将其全部给掉C,或的原因是防止 0 * b而结束循环; 
	{
		if (i < A.size()) t += A[i] * b; //每个数据都乘以b, 
		C.push_back(t % 10);
		t /= 10; //有进位的情况 
	} 
	while (C.size() > 1 && C.back() == 0) C.pop_back(); //解决002这种情况而输出2 
	return C; 
}

int main ()
{
    string a;
    int b = 0;
    cin >> a >> b;
    vector<int> A;
    for (int i = a.size() - 1; i >= 0; i--)
    {
    	A.push_back(a[i] - '0');
	}
	
	auto C = mul(A, b);
	for (int i = C.size() - 1; i >= 0; i--)
	{
		cout << C[i];
	}
	return 0;
} 

```

```c++
//高精度除以低精度 
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
using namespace std;
vector <int> div (vector<int>&A, int b, int &r)
{
	vector<int>C;
	int t = 0;
	r = 0;
	for (int i = A.size() - 1; i >= 0; i--) 
	{
		r = r * 10 + A[i];
		C.push_back(r / b);
		r %= b; 
	} 
	reverse(C.begin(),C.end()); //翻转 
	while (C.size() > 1 && C.back() == 0) C.pop_back(); //解决002这种情况而输出2 
	return C; 
}

int main ()
{
    string a;
    int b = 0;
    int r = 0;
    cin >> a >> b;
    vector<int> A;
    for (int i = a.size() - 1; i >= 0; i--)
    {
    	A.push_back(a[i] - '0');
	}
	
	auto C = div(A, b,r);
	for (int i = C.size() - 1; i >= 0; i--)
	{
		cout << C[i];
	}
	cout << endl << r; //余数 
	return 0;
} 

```

`高精度求阶乘之和`

```c++
#include <iostream>
#include <cstdio>
#include <map>
#include <algorithm>
#include <set>
#include <vector>
#include <cmath>
#include <string>
using namespace std;
int a[2000];
int b[2000];
int c[2000];
void mul(int* a, int d)
{
    int ans = 0;
    for (int i = 1; i <= 1000; i++)
    {
        a[i] = a[i] * d + ans;
        ans = a[i] / 10;
        a[i] %= 10;
    }
}
void add(int* a, int *c)
{
    int ans = 0;
    for (int i = 1; i <= 1000; i++)
    {
        c[i] += a[i] + ans;
        ans = c[i] / 10;
        c[i] %= 10;
    }
}
int main()
{
    int n = 0;
    cin >> n;
    a[1] = 1;
    for (int i = 1; i <= n; i++)
    {
        mul(a, i);
        add(a, c);
    }
    int flag = 0;
    for (int i = 1000; i >= 1; i--)
    {
        if (c[i] != 0) flag = 1;
        if (flag) cout << c[i];
    }
    return 0;
}

```

```c++
//高精乘以高精度
#include <iostream>
#include <cstdio>
#include <map>
#include <algorithm>
#include <set>
#include <vector>
#include <cmath>
#include <cstring>
#include <string>
#include <stack>
using namespace std;
int A[10010], B[10010], c[10010];
char a[10010], b[10010];
int main()
{
	int lena, lenb;
	cin >> a >> b;
	lena = strlen(a);
	lenb = strlen(b);
	for (int i = 1; i <= lena; i++)
	{
		A[i] = a[lena - i] - '0';
	}
	for (int i = 1; i <= lenb; i++)
	{
		B[i] = b[lenb - i] - '0';
	}
	
	for (int i = 1; i <= lena; i++)//计算乘法的时候先不考虑进位，先让i + j - 1; 而i就是我们说的错位相加。
	{
		for (int j = 1; j <= lenb; j++)
		{
			c[i + j - 1] += A[i] * B[j];
		}
	}
	for (int i = 1; i < lena + lenb; i++)//无论数字如何大，数位也不会大于两个乘数的和
	{
		if (c[i] > 9) //大九需要进位
		{
			c[i + 1] += c[i] / 10;
			c[i] %= 10;
		}
		
	}
	int len = lena + lenb;
	while (c[len] == 0 && len > 1) len--;//去除前导0

	for (int i = len ; i >= 1; i--)
	{
		cout << c[i];
	}
	return 0;
}
```

### 十、线性表

#### 1、动态链表

1、创建链表

```c
#include <algorithm>
#include <cstdio>
#include <queue>
#include <vector>
#include <iostream>
#include <stdlib.h>
using namespace std;

struct node{
	int data;//数据
	node* next;
};

//创建链表
node* create(int a[])
{
	node* p, *pre,*head;//pre保存当前结点的前驱结点 
	head = new node;//创建头节点
	head -> next = NULL; //头节点不需要数据 
	pre = head; //记录当前节点
	for(int i = 0; i < 5; i++)
	{
		p = new node; //开创新节点
		p->data = a[i];
		p->next = NULL;
		pre->next = p;
		pre = p; //把pre设为p,作为下节点的前驱结点 
	} 
	return head; 
	 
} 

int main ()
{
	int a[5] = {5,3, 6,1,2};
	node* L = create(a);
    L = L->next;
    while(L != NULL)
    {
    	printf("%d ",L->data );
    	L = L->next;
	}
	return 0;
}
```

2、查找元素

```c
//查找元素x的个数 
int search(node* head, int x)
{
	int count = 0;
	node* p = head->next;
	while(p != NULL)
	{
		if(p->data == x){
			count ++;
		}
		p = p->next;
	} 
	return count;
}
```

3、插入元素

```c
//将元素x插入到位置pos前处
void insert(node* head, int pos, int x)
{
	node* pre = head;
	pre = pre->next;
	for(int i = 0; i < pos - 1; i++)
	{
		pre = pre-> next;
	}
	node* p = new node;
	p->data = x;
	p->next = pre-> next;
	pre->next = p; 
} 

```

4、删除元素

```c
//删除元素x
void del(node* head, int x)
{
	node*pre = head;
	node*p = head->next;
	while(p!=NULL)
	{
		if(p->data == x)
		{
			pre->next = p->next;
			delete(p);
			p = p->next;
		} 
		else{
			pre = p;
			p = p->next;
		}
	}
} 
```

实例：

> 已有a、b两个链表，每个链表中的结点包括学号、成绩。要求把两个链表合并，按学号升序排列，输入：第一行，a、b两个链表元素的数量N、M,用空格隔开。 接下来N行是a的数据 然后M行是b的数据 每行数据由学号和成绩两部分组成
>
> 输出：按照学号升序排列的数据

```c
#include <algorithm>
#include <cstdio>
#include <queue>
#include <vector>
#include <iostream>
#include <stdlib.h>
using namespace std;

struct node{
	int num;
	int score;
	node* next;
};

void insert(node* head, node* b)
{
	node* pre, *p;
	pre = head;
	p = head->next;
	while(p != NULL && p->num < b->num )
	{
		pre = p;
		p = p->next;
	}
	pre->next = b;
	b->next = p;
}

int main ()
{
	node* head, *p;
	head = new node;
	head->next = NULL;
	int n, m;
	cin >> n >> m;
	for(int i = 0; i < n + m; i++)
    {
    	p = new node;
    	p->next = NULL;
    	scanf("%d %d",&p->num, &p->score);
    	insert(head,p);
	}
	for(p = head->next; p != NULL; p = p->next )
	{
		printf("%d %d\n",p->num, p->score );
	}
	return 0;
}
```



#### 2、静态链表

> 原理的实现是靠的hash表，不需要头节点
>
> 具体模板如下

```c++
//建立结构体
struct Node{
    int address;//终点地址
    typename data;//数据域
    int next; //指针域
}node[size];//变量名与结构体名尽量不要一样
//静态链表初始化
for(int i = 0; i < maxn; i++)
{
    node[i].xxx = 0;
}
//p=给出的地址，count = 0;
while(p != -1)//-1代表链表结束
{
    p = node[p].next;
    xxx; //性质
}
```

具体实例

[1、L2-002 链表去重 (25 分)]:https://pintia.cn/problem-sets/994805046380707840/problems/994805072641245184

```c
#include <iostream>
#include <cstdio>
#include <map>
#include <algorithm>
#include <set>
#include <vector>
#include <cmath>
#include <cstring>
#include <string>
using namespace std;
struct node {
	int data;
	int next;
}list[100010];
vector<int>v[2];
int a[100010] = { 0 };
int main()
{
	int first, N = 0;
	cin >> first >> N;
	int temp = 0;
	for (int i = 0; i < N; i++)
	{
		cin >> temp;
		cin >> list[temp].data >> list[temp].next;
		//a[list[temp].data]++;
	}
	int p = first;
	while (p != -1) {
		int count = list[p].data;
		if (a[abs(count)] >= 1) {
			v[1].push_back(p);
		}
		else if (a[abs(count)] == 0) {
			v[0].push_back(p);
		}
		a[abs(count)]++;
		p = list[p].next;
	}
	//cout << "v[0].size() = " << v[0].size() << endl;
	//cout << "v[1].size() = " << v[1].size() << endl;
	int flag = 0;
	for (int i = 0; i < v[0].size(); i++)
	{
		if (flag == 0) {
			printf("%05d %d ", v[0][i],list[v[0][i]].data);
			flag = 1;
		}
		else {
			printf("%05d\n%05d %d ", v[0][i], v[0][i], list[v[0][i]].data);
		}
	}
	cout << "-1" << endl;
	if (v[1].size() != 0) {
		flag = 0;
		for (int i = 0; i < v[1].size(); i++)
		{
			if (flag == 0) {
				printf("%05d %d ", v[1][i], list[v[1][i]].data);
				flag = 1;
			}
			else {
				printf("%05d\n%05d %d ", v[1][i], v[1][i], list[v[1][i]].data);
			}
		}
		cout << "-1" << endl;
	}
	return 0;
}
```

[2、L2-022 重排链表 (25 分)]:https://pintia.cn/problem-sets/994805046380707840/problems/994805057860517888

```c
#include <iostream>
#include <cstdio>
#include <map>
#include <algorithm>
#include <set>
#include <vector>
#include <cmath>
#include <cstring>
#include <string>
using namespace std;
struct node {
	int pre;
	int data;
	int next;
}list[100010];
vector<node>v;
int main()
{
	int N, first;
	cin >> first >> N;
	for (int i = 0; i < N; i++)
	{
		int temp = 0;
		cin >> temp;
		cin >> list[temp].data >> list[temp].next;
		list[temp].pre = temp;
	}
	while (first != -1) {
		v.push_back(list[first]);
		first = list[first].next;
	}
	int l = 0, r = v.size() - 1;
	int count = 0;
	while (1) {
		if (count + 1 == v.size() ){
			printf("%05d %d -1", v[r].pre, v[r].data);
			break;
		}
		printf("%05d %d %05d\n", v[r].pre, v[r].data, v[l].pre);
		count++;
		if (count + 1 == v.size()) {
			printf("%05d %d -1", v[l].pre, v[l].data);
			break;
		}
		printf("%05d %d %05d\n", v[l].pre, v[l].data, v[r - 1].pre);
		count++;
		l++, r--;
	}
	return 0;
}
```

[3、1075 链表元素分类 (25 分)]:https://pintia.cn/problem-sets/994805260223102976/problems/994805262953594880

```c
#include <iostream>
#include <cstdio>
#include <map>
#include <algorithm>
#include <set>
#include <vector>
using namespace std;
struct node {
    int data;
    int next;
}list[100000];

vector<int>v[3]; //总共有三种元素，小于【0，k】为在首位置，在[0,k]在中间，大于k在后
int main()
{
    int first = 0, K = 0, N = 0;
    cin >> first >> N >> K;
    for (int i = 0; i < N; i++)
    {
        int temp = 0;
        cin >> temp;
        cin >> list[temp].data >> list[temp].next;
    }
    int p = first;
    while (p != -1)
    {
        int count = list[p].data;
        if (count < 0) v[0].push_back(p);
        else if (count >= 0 && count <= K) v[1].push_back(p);
        else if (count > K) v[2].push_back(p);
        p = list[p].next;
    }
    int flag = 0;
    for (int i = 0; i < 3; i++)
    {
        for (int j = 0; j < v[i].size(); j++)
        {
            if (flag == 0)
            {
                printf("%05d %d ", v[i][j], list[v[i][j]].data);
                flag = 1;
            }
            else {
                printf("%05d\n%05d %d ", v[i][j], v[i][j], list[v[i][j]].data);
            }
        }
    }
    cout << "-1" << endl;
    return 0;
}

```

[4、1025 反转链表 (25 分)]:https://pintia.cn/problem-sets/994805260223102976/problems/994805296180871168

```c
#include <iostream>
#include <cstdio>
#include <map>
#include <algorithm>
#include <set>
#include <vector>
#include <cmath>
#include <cstring>
#include <string>
using namespace std;
int main()
{
	int N = 0, K = 0,first = 0;
	cin >> first >> N >> K;
	int data[100010] = { 0 }, next[100010] = { 0 }, list[100010] = { 0 };
	int temp = 0;
	for (int i = 0; i < N; i++)
	{
		cin >> temp;
		cin >> data[temp] >> next[temp];
	}
	int count = 0;
	while (first != -1)
	{
		list[count++] = first;
		first = next[first];
	}
	for (int i = 0; i <(count - (count % K)); i += K)
	{
		reverse(list + i, list + i + K);
	}
	for (int i = 0; i < count - 1; i++)
	{
		printf("%05d %d %05d\n", list[i], data[list[i]], list[i + 1]);
	}
	printf("%05d %d -1", list[count - 1], data[list[count - 1]]);
	return 0;
}
```



#### 3、中缀表达式转后缀表达式，并计算结果

> 从头到尾读取中缀表达式的每个对象，对不同的对象按不同的情况处理。（符号入栈，出栈，数据直接输出，运算符用栈，把后缀表达式用数组或队列存放）
>
> 运算数：直接输出
>
> 左括号：压入栈
>
> 右括号：将栈顶的运算符弹出并输出，直到遇到左括号（括号出栈但是不输出）
>
> 运算符；如果优先级大于栈顶运算符时，则把他压入栈；若优先级小于等于栈顶运算符的优先级，将栈顶运算符弹出并输出；再比较新的栈顶运算符，直到该运算符大于栈顶运算符优先级为止，然后将该运算符压入栈；
>
> 若个对象处理完毕，则把堆栈中存留的运算符一并输出。

> 计算后缀表达式：从左到右一次扫描后缀表达式，如果是操作数，就压入栈，如果是操作符，就连续弹出两个操作数（后弹出的是第一个操作数，第一个弹出的是第二个操作数），然后进行操作符的操作，生成新的操作数压入栈中。反复直到后缀表达式扫描完毕，这是栈中只有一个数，就是最终答案。（除法可能导致浮点数，因此将其设成浮点型变量）

```c++
#include <iostream>
#include <cstdio>
#include <map>
#include <algorithm>
#include <set>
#include <vector>
#include <cmath>
#include <cstring>
#include <string>
#include <stack>
#include<queue>
using namespace std;
struct node {
	double num; //操作数
	char op; //运算符
	bool flag; //ture 为操作数，false为运算符
};
string str;//输入表达式
stack <node> s;//堆栈，用来云中缀表达式的运算符，和后缀表达式的操作数
queue<node> q; //用来表示后缀表达式
map<char, int> mp; //运算符的优先级

void Change()
{
	double num;
	node temp;
	for (int i = 0; i < str.length();)
	{
		if (str[i] >= '0' && str[i] <= '9') {
			temp.flag = true;//标记是数字
			temp.num = str[i++] - '0';
			while (i < str.length() && str[i] >= '0' && str[i] <= '9') {
				temp.num = temp.num * 10 + (str[i] - '0');
				i++;
			}
			q.push(temp);//操作数压入后缀表达式队列中
		}
		else {
			temp.flag = false;
			//只要操作符栈顶元素比该操作符优先级高，就把该操作符栈顶元素弹出到后缀表达式队列中国
			while (!s.empty() && mp[str[i]] <= mp[s.top().op]) {
				q.push(s.top());
				s.pop();
			}
			temp.op = str[i];
			s.push(temp);//把该操作符压入操作符栈中
			i++;
		}
	}
	while (!s.empty()) {
		q.push(s.top());
		s.pop();
	}
	
}

double Cal() //计算后缀表达式
{
	double temp1, temp2;
	node cur, temp;
	while (!q.empty()) //对列不为空
	{
		cur = q.front();
		q.pop();
		if (cur.flag) s.push(cur);//如果是操作数，则入栈
		else {
			temp2 = s.top().num;//弹出第二个操作数
			s.pop();
			temp1 = s.top().num;
			s.pop();
			temp.flag = true; //临时记录操作数
			if (cur.op == '+') temp.num = temp1 + temp2;
			else if (cur.op == '-') temp.num = temp1 - temp2;
			else if (cur.op == '*') temp.num = temp1 * temp2;
			else temp.num = temp1 / temp2;
			s.push(temp); //压入栈
		}
	}
	return s.top().num;//栈顶元素几位后缀表达式
}
int main()
{
	mp['+'] = mp['-'] = 1;
	mp['*'] = mp['/'] = 2;
	while(getline(cin, str) && str != "0")
	{
		for (int i = 0; i < str.length();i++)
		{
			if (str[i] == ' ') str.erase(i,1); //去除多余空格
		}
		while (!s.empty()) s.pop();//初始化栈堆
		Change();//将中缀表达式转为后缀表达式
		node t;
		/*
		while(!q.empty())
		{
			t = q.front();
			if (t.flag) {
				printf("%.f ", t.num);
			}
			else {
				printf("%c ", t.op);
			}
			q.pop();
		}
		*/
		printf("%.2f\n", Cal()); //计算后缀表达式
	}
	return 0;
}
```

#### 4、队列应用

```markdown
约瑟夫问题：n个人围成圈，从第一个人开始报数，直到报出m，该人出列，下个人接着从1开是报
提供两种思路（一、队列法，二、纯模拟法）
一、队列法；先把所有数据入队，然后设一变量从1开始，当遇到m,就删去当前队列首元素，并让该变量等于1，当没遇到m，就不断让该变量自加，删除队首元素，并让被删除的队首元素压入队列到队尾，完成循环出队入队
二、纯模拟法：用有n个人，直到有n个人完全出去才算结束，这是外循环，内循环用for循环，让m != 从一开始自加的变量，为循环条件，表达式为，如果该数值出现过，就让其变为-1，并结束此循环，没出现，开变量i增加到那个地步，当为n时，用 i%= m;达到循环效果，如果变量等于m,就让次数据输出，并让器改值变为-1，表示用过该数据了。
```

```c++
//模拟，
#include <iostream>
#include <cstdio>
#include <map>
#include <algorithm>
#include <set>
#include <vector>
#include <cmath>
#include<stack>
#include<queue>
#include <cstring>
#include <string>
using namespace std;
int a[110] = { 0 };

int main()
{
	int n = 0, m = 0;
	cin >> n >> m;
	int num = n,last = 1;//last是用来记录下一个人的位置
	while (num)
	{
		int count = 0;
		for (int i = last; count != m; i++)
		{
			if (i > n) i %= n;
			if (a[i] == -1) continue;
			++count;
			if (count == m)
			{
				a[i] = -1;
				printf("%d ", i);
				last = i + 1;
				num--;//人数少一个
				break;
			}
		}
	}
	return 0;
}
```

```c++
//队列
#include <iostream>
#include <cstdio>
#include <map>
#include <algorithm>
#include <set>
#include <vector>
#include <cmath>
#include<stack>
#include<queue>
#include <cstring>
#include <string>
using namespace std;
queue<int>q;

int main()
{
	int n = 0, m = 0;
	cin >> n >> m;
	int ans = 1;
	for (int i = 1; i <= n; i++)
	{
		q.push(i);
	}
	while (!q.empty())
	{
		if (ans == m)
		{
			ans = 1;
			int temp = q.front();
			q.pop();
			printf("%d ", temp);
		}
		else {
			ans++;
			q.push(q.front());
			q.pop();
		}
	}
	return 0;
}
```

#### 5、双向链表

[队列安排]:https://www.luogu.com.cn/problem/P1160

```c++
//队列
#include <iostream>
#include <cstdio>
#include <map>
#include <algorithm>
#include <set>
#include <vector>
#include <cmath>
#include<stack>
#include<queue>
#include <cstring>
#include <string>
using namespace std;
struct node {
	int r;
	int l;
	int flag;
}k[100010];

int main()
{
	int n, m, x, y, count = 0;
	cin >> n;
	k[1].l = 0, k[1].r = -1, k[1].flag = 1;
	k[0].r = 1, k[0].flag = 1;//带头结点的双向链表
	for (int i = 2; i <= n; i++) //结点的插入
	{
		cin >> x >> y;
		k[i].flag = 1;
		if (y == 0) //往右插入
		{
			k[k[x].l].r = i; //双向链表的插入过程
			k[i].l = k[x].l;
			k[i].r = x;
			k[x].l = i;
		}
		else {
			if (k[x].r != -1)
			{
				k[k[x].r].l = i;
				k[i].r = k[x].r;
				k[i].l = x;
				k[x].r = i;
			}
			else {
				k[x].r = i;
				k[i].l = x;
				k[i].r = -1;
			}
		}
	}
	cin >> m;
	for (int i = 0; i < m; i++)//删除的过程
	{
		cin >> x;
		if (k[x].flag == 1)//该数据第一次出现
		{
			k[x].flag = 0;
			k[k[x].l].r = k[x].r;
			k[k[x].r].l = k[x].l;
			count++;//删除数据的个数
		}
	}
	//cout << "count = " << count << endl;
	int pr = 0, p = k[pr].r;
	for (int i = 0; i < n - count; i++)
	{
		cout << p << " ";
		pr = p;
		p = k[pr].r;
	}
	return 0;
}
```

#### 6、栈的应用

[出栈序列统计]:http://codeup.cn/problem.php?cid=100000608&pid=4

```markdown
递归思路（dfs）,递归实现
1.递归参数分别是栈中元素个数，进栈指针，出栈指针
2.递归边界是，当进栈指针和出栈指针同时满的时候（此时也是一种方案形成的时候）
3.递归子式；当进栈指针小于n时，栈元素数量增加一，进栈指针加一，出栈指针不变，出栈指针小于n时，栈中元素减一（且栈非空，否则不能减），出栈指针加一，进栈指针不变。
```

```c
#include <iostream>
#include <cstdio>
#include <map>
#include <string>
#include <queue> 
#include <algorithm>
#include<cmath>
using namespace std;
int n = 0, ans = 0;
void dfs(int num, int push, int pop)
{
	if (push == n && pop == n)
	{
		ans++;
		return;
	}
	if (push < n ) {
		dfs(num + 1, push + 1, pop);
	}
	if (pop < n && num > 0) { //栈中元素非空
		dfs(num - 1, push, pop + 1);
	}
}
int main()
{
	cin >> n;
	dfs(0, 0, 0);
	printf("%d", ans);
	return 0;
}
```

[验证栈序列]:https://www.luogu.com.cn/problem/P4387

```c++
#include <iostream>
#include <cstdio>
#include <map>
#include <algorithm>
#include <set>
#include <vector>
#include <cmath>
#include<stack>
#include<queue>
#include <cstring>
#include <string>
using namespace std;
stack<int>s;
int main()
{
	int q = 0, n = 0;
	cin >> q;//询问次数
	while (q--)
	{
		cin >> n;
		int a[100010], b[100010], count = 1; //输 入栈序列，待验证序列，队b序列计数
		for (int i = 1; i <= n; i++) cin >> a[i];
		for (int i = 1; i <= n; i++) cin >> b[i];
		for (int i = 1; i <= n; i++)
		{
			s.push(a[i]); //入栈
			while (s.top() == b[count]) //相等的话,栈顶元素与当前元素同时出栈
			{
				s.pop();
				count++;//count到b的下一个元素
				if (s.empty()) break;
			}
		}
		if (s.empty()) cout << "Yes" << endl;
		else cout << "No" << endl;
		while (!s.empty()) s.pop();//清空栈元素
	}
	return 0;
}
```

### 十一、递归

```markdown
分治思想；1、把一个庞大的问题，分解成若干子问题； 2、决绝每个小问题、3、合并
递归；反复调用自己的函数，是范围缩小，解决问题后，在一次往上返回
递归重要思想；1、寻找递归边界，并明确找到递归边界后做什么
             2找递归式，找到递归式后做什么
```

全排列问题

```c
#include <iostream>
#include <cstdio>
#include <map>
#include <algorithm>
#include <set>
#include <vector>
#include <cmath>
#include<stack>
#include<queue>
#include <cstring>
#include <string>
using namespace std;
int p[100]; //存放 当前排列 
int n = 0;
int hashtable[100] = {false}; //判段某数字在当前位是否出现过
 
void dfs(int index) //index 处理当前排列 
{
	if(index == n + 1) //递归边界，且还要直到做什么
	{
		for(int i = 1; i <= n; i++) //找到递归边界后，知道了存放排列值满员了，输出即可
		{
			if(i == 1) cout << p[i];
			else cout << " " << p[i];
		} 
		cout << endl;
		return ;//开始往回返 
	}
	for(int i = 1; i <= n; i++) //寻找1 ~ n开头的排列式子
	{
		if(hashtable[i] == false) //表明该数字还没存入p中
		{
			hashtable[i] = true; 
			p[index] = i;
			dfs(index + 1); //处理index + 1 号排列 
			hashtable[i] = false; //处理完p[index]位x的子问题，还原状态。 
		} 
		
	} 
}
int main()
{
	
	cin >> n;
	dfs(1);
    return 0;
}
```

n！ = （n - 1)! * n;

```c
#include <iostream>
#include <cstdio>
#include <map>
#include <algorithm>
#include <set>
#include <vector>
#include <cmath>
#include<stack>
#include<queue>
#include <cstring>
#include <string>
using namespace std;
int dfs(int n)
{
	if(n == 1) return 1;
	else{
		return dfs(n - 1) * n;
	}
} 
int main()
{
    int n = 0;
    cin >> n;
    printf("%d",dfs(n));
    return 0;
}
```

斐波那契数列

```c
#include <iostream>
using namespace std;
int fib(int n)
{
	if(n == 1 || n == 0) return 1;
	else{
		return fib(n- 1) + fib(n - 2);
	}
} 
int main()
{
    int n = 0;
    cin >> n;
    printf("%d",fib(n));
    return 0;
}
```

汉诺塔问题

```c
#include <cstdio>
#include<iostream>
using namespace std;
int h(int n)
{
	if (n == 1) return 1;
	else {
		return (2 * h(n - 1) + 1);
	}
}
int main()
{
	int n = 0;
	cin >> n;
	cout << h(n);
	return 0;
}
```

汉诺塔问题||

```c
#include <cstdio>
#include<iostream>
using namespace std;
int count = 0; //每移动一次，就是一步，
void h(int n, char a, char b, char c) //由a经由b,到c
{
	if (n == 1)
	{
		cout << n << " " << a << " -> " << c << endl; //只有一个盘子，直接由a飞到c即可
		count++;
	}
	else {
		h(n - 1, a, c, b);//将n - 1个盘子从a处，经c移动到b处
		cout << n << " " << a << " -> " << c << endl;//那么第n个盘子可以直接从a到c处，因为n- 1个盘子已经飞走了
		count++;
		h(n - 1, b, a, c); //那么n- 1的盘子，从出发，经由a，到c处，完成移动到c盘上。
	}   
	
}
int main()
{
	int n = 0;
	cin >> n;
	h(n, 'A', 'B', 'C');//n个盘子在a柱子上，把其全部移动到c柱子上的步骤输出
	cout << count;
	return 0;
}
```

汉诺塔问题|||

```c
#include <cstdio>
#include<iostream>
using namespace std;
int count = 0;
int h(int n) 
{
	if (n == 1) return 2;
	else {
		return (h(n - 1) + 1 + h(n - 1) + 1 + h(n - 1)); //以n= 2来推算关系
	}
}
int main()
{
	int n = 0;
	cin >> n;
	cout << h(n);
	return 0;
}
```

### 十二、递推

重点在于找规律，根据前几项找到规律（说白了，就是根据前几项找出规律）

斐波那契数列

```c
//数组法
#include <cstdio>
#include<iostream>
using namespace std;
int a[1000] = { 0 }; //数组法
int main ()
{   
	int n = 0;
	cin >> n;
	a[0] = 1, a[1] = 1;
	for (int i = 2; i <= n; i++)
	{
		a[i] = a[i - 1] + a[i - 2];
	}
	cout << a[n];
	return 0;
}
//非数组法
#include <cstdio>
#include<iostream>
using namespace std;
int main ()
{   
	int n = 0;
	cin >> n;
	int fn = 0, fn_1 = 1, fn_2 = 1;
	for (int i = 2; i <= n; i++)
	{
		fn = fn_1 + fn_2;
		fn_2 = fn_1;
		fn_1 = fn;
	}
	cout << fn;
	return 0;
}
```

汉诺塔问题

```c
//fn = f(n - 1) + 1 + f(n - 1); //这是递推的公式
#include <cstdio>
#include<iostream>
using namespace std;
int main()
{
	int n = 0;
	cin >> n;
	int fn = 0, fn_1 = 1;
	for (int i = 2; i <= n; i++)
	{
		fn = 2 * fn_1 + 1; //移动的次数
		fn_1 = fn;
	}
	cout << fn;
	return 0;
}
```

爬楼梯

```markdown
爬楼梯，一部可以爬1个，也可以爬2个，或者3个，请问爬到第n个楼梯，一共有多少种走法？
fn = fn_1 + fn_2 + fn_3;
这是必须知道fn_1, fn_2, fn_3的值
```

```c
#include <cstdio>
#include<iostream>
using namespace std;

int main()
{
	int n = 0;
	cin >> n;
	int fn = 0, fn_1 = 4, fn_2 = 2, fn_3 = 1;
	if (n == 1) { cout << 1; return 1; }
	else if (n == 2) { cout << 2; return 2; }
	else if (n == 3) { cout << 3; return 4; }
	for (int i = 4; i <= n; i++)
	{
		fn = fn_1 + fn_2 + fn_3;
		fn_3 = fn_2;
		fn_2 = fn_1;
		fn_1 = fn;
	}
	cout << fn;
	return 0;
}
```

### 十三、二分查找

> 定义：在一段有序的序列里面，通过不断缩小区间，来找到，等于/大于等于/大于某元素的位置（下标）

```c
//找位置在区间里存在的位置
#include <iostream>
#include <cstdio>
#include <map>
#include <algorithm>
#include <set>
#include <vector>
#include <cmath>
#include <string>
using namespace std;
int binarySearch(int a[], int left, int right, int x)
{
	int mid = 0; //mid为left和right的中点
	while (left <= right) //当取等号时，说明找到的是唯一的值
	{
		mid = (left + right) / 2;
		if (a[mid] == x) return mid;
		else if (a[mid] > x) right = mid - 1; //往左区间[left,mid - 1]找
		else if (a[mid] < x) left = mid + 1; //往右区间上找
	}
	return -1; //找不到返回-1
}
int main()
{
	int n = 10;
	int a[10] = { 1,2,3,4,6,7,8,10,12,13 };//区间[0,n-1]
	//找到要查找到元素在序列中的位置
	int x = binarySearch(a, 0, n - 1, 3);//查元素3
	int y = binarySearch(a, 0, n - 1, 12);//查元素12
	cout << x << " " << y << endl;
	return 0;
}
```

```c
//找第一个大于等于要查找到位置
#include <iostream>
#include <cstdio>
#include <map>
#include <algorithm>
#include <set>
#include <vector>
#include <cmath>
#include <string>
using namespace std;
int binarySearch(int a[], int left, int right, int x)
{
	int mid = 0; //mid为left和right的中点
	while (left < right) //当取等号时，说明找到的是唯一的值
	{
		mid = (left + right) / 2;
	    if (a[mid] >= x) right = mid ; //往左区间[left,mid ]找
		else if (a[mid] < x) left = mid + 1; //往右区间上找
	}
	return left; //因为right和left最后都一样，所以返回谁都一样
}
int main()
{
	int n = 10;
	int a[10] = { 1,2,3,3,6,7,8,10,12,12 };//区间[0,n-1]
	//找到要查找到元素在序列中的位置
	int x = binarySearch(a, 0, n - 1, 3);//查元素3
	int y = binarySearch(a, 0, n - 1, 12);//查元素12
	cout << x << " " << y << endl;
	return 0;
}
```

```c
//找出在有序序列中1第一个大于要查找到位置
#include <iostream>
#include <cstdio>
#include <map>
#include <algorithm>
#include <set>
#include <vector>
#include <cmath>
#include <string>
using namespace std;
int binarySearch(int a[], int left, int right, int x)
{
	int mid = 0; //mid为left和right的中点
	while (left < right) //当取等号时，说明找到的是唯一的值
	{
		mid = (left + right) / 2;
	    if (a[mid] > x) right = mid ; //往左区间[left,mid ]找
		else left = mid + 1; //往右区间上找
	}
	return left; //因为right和left最后都一样，所以返回谁都一样
}
int main()
{
	int n = 10;
	int a[10] = { 1,2,3,3,6,7,8,10,12,12 };//区间[0,n-1]
	//找到要查找到元素在序列中的位置
	int x = binarySearch(a, 0, n - 1, 3);//查元素3
	cout << x << endl;
	return 0;
}
```

### 十四、贪心

```markdown
找到一种方案，是当前的收益最大。

解决贪心问题的基本步骤；
1、将原问题分解为子问题
2、找出贪心策略
3、得到每一个子问题的最优解
4、将所有子问题的最优解的集合构成为原问题的一个解
实质也是个递归的过程，贪心算法根本在于贪心策略的解决。而贪心策略一定是最优的（找到一种算法，使得当前收益最大），需要大量的积累，而不是光靠记模板
如果找不到正确的贪心策略，可以猜，所以需要再这方面进行大量的练习（因为跨度比较大，考虑到数学建模，和数学归纳，因此没有固定的模板，只能通过刷题来提升这种算法）
```

不过有几个题型可以参考

[1、P1223 排队接水]:https://www.luogu.com.cn/problem/P1223

```c
//这道题的主要思路是：使每个人的平均等待时间都最少，那么就让其等待的时间长度从小到大排序即可,从而找出最优方案。
#include <iostream>
#include <vector>
#include <cstdio>
#include <algorithm>
#include <cstring>
#include <queue>
#include <map>
#include <stack>
#include <stdlib.h>
using namespace std;

const int maxn = 1010;
int n = 0;
int a[maxn];
int b[maxn] = { 0 };
bool visit[maxn] = { false };
int main()
{
	cin >> n;
	int t;
	double sum = 0,count = 0;
	for (int i = 1; i <= n; i++)
	{
		cin >> t;
		b[i] = t;
		a[i] = b[i];
	}
	sort(b + 1, b + n + 1);
	for (int i = 1; i <= n; i++)
	{
		for (int j = 1; j <= n; j++)
		{
			if (visit[j] == false)
			{
				if (b[i] == a[j])
				{
					visit[j] = true;
					printf("%d ", j);
					break;
				}
			}
		}
	}
	cout << endl;
	for (int i = 2; i <= n; i++)
	{
		sum += b[i-1];//等待时间嘛
		count += sum;
	}
	count = count / (n * 1.0);
	printf("%.2lf", count);
	return 0;
}
```

[2、P1803 凌乱的yyy / 线段覆盖]:https://www.luogu.com.cn/problem/P1803

```c
//比赛的开始和结束的时间，求解最多能参加多少场比赛。因为所有的比赛时间点在同一个时间轴上，所以结束时间越早的比赛就是必须参加的，那么我们可以通过结束时间点进行从大到小的排序。由于一个时间点只能参加一个比赛，那么就需要剔除时间上有交叉的比赛（剔除比赛时间较长的一个）。可以使用一个pos记录上一次参加比赛的结束时间，下一场比赛是否参加，只需要将pos与要参加的比赛的开始时间进行比较。只有小于等于的情况才参加
#include <iostream>
#include <vector>
#include <cstdio>
#include <algorithm>
#include <cstring>
#include <queue>
#include <map>
#include <stack>
#include <stdlib.h>
using namespace std;

const int maxn = 1010;
struct node {
	int start;//比赛开始的时间
	int end;//比赛结束的时间
}bisai[1000010];

bool cmp(node a, node b)
{
	return a.end < b.end; //以比赛结束的时间从大到小进行排序
}
bool visit[maxn] = { false };
int main()
{
	int n = 0;
	cin >> n;
	for (int i = 1; i <= n; i++)
	{
		cin >> bisai[i].start >> bisai[i].end;
	}
	sort(bisai + 1, bisai + 1 + n, cmp);
	int ans = 0; //表示可以参加多少场比赛
	int pos = 0; //记录上一场比赛参加的时间，一当场结束的时间跟下一场比赛做对比

	for (int i = 1; i <= n; i++)
	{
		if (pos <= bisai[i].start)
		{
			ans++;
			pos = bisai[i].end;
		}
	}
	cout << ans;
	return 0;
}
```

[3、P3817 小A的糖果]:https://www.luogu.com.cn/problem/P3817

```c
#include <iostream>
#include <vector>
#include <cstdio>
#include <algorithm>
#include <cstring>
#include <string>
#include <cmath>
using namespace std;
const int maxn = 100010;
int a[maxn] = { 0 };
int main()
{
	int n; //果盘数量
	int x; //糖果剩余数量的最大值
	long long int sum = 0; //吃掉糖果剩余数量的最小值
	cin >> n >> x;
	cin >> a[1];
	//对开始的果盘处理
	if (a[1] > x)
	{
		sum += a[1] - x;
		a[1] = x;//大于x的话，达到该值，就满足要求了
	}
	for (int i = 2; i <= n; i++)
	{
		cin >> a[i];
	}
	//处理剩余果盘
	for (int i = 2; i <= n; i++)
	{
		if (a[i] + a[i - 1] > x)
		{
			sum += (a[i] + a[i - 1] - x); //a[i] + a[i - 1] - x; 就等于吃下的糖果，剩余的一定等于3
			a[i] = x - a[i - 1];
		}
	}
	cout << sum;
	return 0;
}
```

[4、P1478 陶陶摘苹果（升级版]:https://www.luogu.com.cn/problem/P1478

```c
#include <iostream>
#include <vector>
#include <cstdio>
#include <algorithm>
#include <cstring>
#include <string>
using namespace std;
const int maxn = 5010;
int n, s, a, b;
struct node {
	int high;
	int li;
}c[maxn];

bool cmp(node a, node b)
{
	return a.li < b.li;
}
int main()
{
	cin >> n >> s;
	cin >> a >> b;
	int d = a + b;
	int temp = 0,end = 0,sum = 0;
	for (int i = 0; i < n; i++)
	{
		cin >> temp >> end;
		c[i].high = temp;
		c[i].li = end;
	}
	sort(c, c + n, cmp);
	for (int i = 0; i < n; i++)
	{
		cout << c[i].high << " "<< c[i].li << endl;
	}
	for (int i = 0; i < n; i++)
	{
		
		if (c[i].high <= d) {
			s -= c[i].li;
			if (s < 0) { break; }
			sum++;
		}
	}
	cout << sum << endl;
	return 0;
}
```

[5、5、P1094  纪念品分组]:https://www.luogu.com.cn/problem/P1478

```c
#include <iostream>
#include <vector>
#include <cstdio>
#include <algorithm>
#include <cstring>
#include <string>
using namespace std;
const int maxn = 30010;
int w, n, g;
int a[maxn] = { 0 };
bool visit[maxn] = { false };

bool cmp(int a, int b)
{
	return a < b;
}
int main()
{
	cin >> w;
	cin >> g;
	for (int i = 0; i < g; i++)
	{
		cin >> a[i];
	}
	sort(a, a + g, cmp);
	int count = 0; //组别的数量
	for (int i = 0; i < g / 2; i++)
	{
		for (int j = g - 1 - i; j >= i; j--)
		{
			if (visit[j] == false) {
				if (a[i] + a[j] <= w) {
					count++;
					visit[i] = true;
					visit[j] = true;
					break;
				}
			}
		}
		
	}
	for (int i = 0; i < g; i++)
	{
		if (visit[i] == false) count++;
	}
	cout << count;
	return 0;
}
```



### 十五、dfs,bfs

dfs简单的几个模板

[perkey](https://www.luogu.com.cn/problem/P2036)

```c++
#include <iostream>
#include <algorithm>
#include <queue>
#include <cmath>
using namespace std;
int n = 0,ans = 0x7FFFFFFF;
int a[15],b[15];
//要知道该题的配料是有可选和不可选的两种情况 
void dfs(int i, int x, int y) //x表示酸度，y表示苦度，i表示添加到第几种配料 
{
	if(i > n) //在配料选完的情况下
	{
		if(x == 1 && y == 0) return; //筛除清水
		ans = min(abs(x - y), ans) ; //找出最小值 
		return;
	} 
	//每种配料都有选则和不选两种情况 
	dfs(i + 1, a[i] * x, b[i] + y);
	dfs(i + 1, x, y); 
}

int main()
{
	cin >> n;
	for(int i = 1; i <= n; i++)
	{
		scanf("%d %d",&a[i],&b[i]);
	}
	dfs(1,1,0);//酸度是乘积必须为1，苦度为和，是0
	printf("%d",ans); 
	return 0; 
} 

```

迷宫问题

```c
#include <iostream>
#include <cstdio>
#include <map>
#include <string>
#include <queue> 
#include <algorithm>
#include<cmath>
using namespace std;
struct node {
	int x;
	int y;
}r[1000];//路径


int dx[4] = { 0,-1,0,1 }, dy[4] = { -1,0,1,0 };//注意例题的遍历方向
int mp[20][20]; //地图
int flag = 0; //判断有没有路可走
int start_x = 0, start_y = 0;//起始坐标
int end_x = 0, end_y = 0; //重点坐标
int m, n; //迷宫大小

void dfs(int x, int y, int step)
{
	if ((end_x == x && end_y == y))//递归边界，找到一条出路
	{
		for (int i = 0; i < step; i++)//输出路径
		{
			printf("(%d,%d)->", r[i].x, r[i].y);
		}
		printf("(%d,%d)\n", x, y);
		flag = 1;
		return;
 	}
	r[step].x = x;
	r[step].y = y; //更新路径
	for (int i = 0; i < 4; i++)
	{
		if (mp[x + dx[i]][y + dy[i]] == 1) //有路可走
		{
			mp[x][y] = 0;//该路走过，避免走重
			dfs(x + dx[i], y + dy[i], step + 1); //遍历该方向，朝着这个方向搜索看看有没有出路
			mp[x][y] = 1;//回到该方向，在为遍历其他方向寻找出路
		}
	}
}

int main()
{
	cin >> m >> n; //输入地图大小
	for (int i = 1; i <= m; i++)
	{
		for (int j = 1; j <= n; j++)
		{
			cin >> mp[i][j];//输入地图
		}
	}
	cin >> start_x >> start_y;
	cin >> end_x >> end_y;
	dfs(start_x, start_y,0);
	if (flag == 0)
	{
		printf("-1");
	}
	return 0;
}
```

n皇后问题

```c
#include <iostream>
#include <cstdio>
#include <map>
#include <string>
#include <queue> 
#include <algorithm>
#include<cmath>
using namespace std;
int n = 0,ans = 0;
int p[14];
bool hashtable[14] = { false };
void dfs(int index)
{
	if (index == n + 1)
	{
		//cout << "ans = " << ans << endl;
		int flag = 1;
		for (int i = 1; i <= n; i++)
		{
			for (int j = i + 1; j <= n; j++)
			{
				if (abs(i - j) == abs(p[j] - p[i])) //对角线上都有皇后，显然通过全排列，相同的行列上不可能都有皇后
				{
					flag = 0;
					//cout << "flag = " << flag << endl;
				}
			}
		}
		if (flag) {
			ans++;
			for (int i = 1; i <= n; i++)
			{
				if (i == 1) printf("%d", p[i]);
				else printf(" %d", p[i]);
			}
			cout << endl;
		}
		return;
	}

	for (int i = 1; i <= n; i++) //n的全排列
	{
		if (hashtable[i] == false) {
			p[index] = i;
			hashtable[i] = true;
			dfs(index + 1);
			hashtable[i] = false;
		}
	}
}
int main()
{
	cin >> n;
	dfs(1);
	if (ans == 0) printf("no solute!");
	return 0;
}
//回溯算法
//回溯算法
#include <iostream>
#include <cstdio>
#include <map>
#include <string>
#include <queue> 
#include <algorithm>
#include<cmath>
using namespace std;
int n = 0;
int p[20];
bool hashtable[20] = { false };
int ans = 0;
void dfs(int index) //全排列的方式
{
	if (index == n + 1) //递归边界
	{
		ans++;//能达到这里一定是合法的
		if (ans <= 3)
		{
			for (int i = 1; i <= n; i++)
			{
				if (i == 1) printf("%d", p[i]);
				else printf(" %d", p[i]);
			}
			cout << endl;
		}
		return;
	}

	for (int i = 1; i <= n; i++) //递归式,第x行
	{
		if (hashtable[i] == false)//第x行还没有皇后
		{
			bool flag = true; //该flag表示该行的皇后不会和之前的皇后产生冲突
			for (int pre = 1; pre < index; pre++)
				//第index列皇后行号为i,第pre列皇后的行号为p[pre]
			{
				if (abs(index - pre) == abs(p[pre] - i))
				{
					flag = false; //冲突了，没有进行下去的必要了
				    break;
				}
			}
			if (flag) {
				p[index] = i;
				hashtable[i] = true;
				dfs(index + 1);
				hashtable[i] = false;
			}
		}
	}
}
int main()
{
	scanf("%d", &n);
	dfs(1);
	printf("%d", ans);
	return 0;
}
```

出栈序列个数

```c
#include <iostream>
#include <cstdio>
#include <map>
#include <string>
#include <queue> 
#include <algorithm>
#include<cmath>
using namespace std;
int n = 0, ans = 0;
void dfs(int num, int push, int pop)
{
	if (push == n && pop == n)
	{
		ans++;
		return;
	}
	if (push < n ) {
		dfs(num + 1, push + 1, pop);
	}
	if (pop < n && num > 0) { //栈中元素非空
		dfs(num - 1, push, pop + 1);
	}
}
int main()
{
	cin >> n;
	dfs(0, 0, 0);
	printf("%d", ans);
	return 0;
}
```

从n个数中取r个数问题

```c
#include <cstdio>
#include<iostream>
#include <algorithm>
#include<string>
#include<vector>
using namespace std;
int n = 0, r = 0;
int a[22] = { 0 };
void dfs(int index, int count)
{
	a[count] = index;
	if (count == r) {
		for (int i = 1; i <= r; i++)
		{
			if (i == 1) printf("%d",a[i]);
			else printf(" %d",a[i]);
		}
		printf("\n");
		return;
	}
	for (int i = index + 1; i <= n; i++)
	{
		dfs(i, count + 1);
	}
}
int main()
{
    scanf("%d %d",&n,&r);
	dfs(0,0);
	return 0;
}
```

[典型bfs问题]:http://codeup.cn/problem.php?cid=100000609&pid=1

```markdown
此题的陷阱就是不能判重，所有走过的点都可以再走一遍，而且我们只需要保证走满八步不会死掉就可以活下去（或者走到终点），因为八步以后石头会全部掉下去，
广度搜索八个方向，切记还有原点不动的方向，如果该搜索的方向没有越界，上一步搜索的不是大石头，并且这一步搜索的不是大石头，就可以去判断是否到达了终点或者是是否走够了八步。
```

```c
//知道上一步的坐标怎么计算，当前坐标 - 当前行走的步数
#include <iostream>
#include <cstdio>
#include <map>
#include <string>
#include <queue> 
#include <algorithm>
#include<cmath>
using namespace std;
struct node {
	int x; 
	int y;
	int step;
}cur,temp; //一个表示队列第一个元素，一个表示队列临时存储元素 
int flag = 0;
char a[8][9];//8*8的盘，九个方向 
int dx[] = { 0,1,1,1,-1,-1,-1,0,0 }; //搜索九个方向
int dy[] = { 0,0,1,-1,0,1,-1, 1,-1};

bool check(int x,int y) //判断走的是否越界
{
	if(x >= 8 || x < 0 || y >= 8 || y < 0)
	{
		return false;
	}
	return true;
} 

void bfs()
{
	cur.x = 7;
	cur.y = 0; //从左下角开始走
	cur.step = 0;
	queue<node>q;
	q.push(cur);
	while(!q.empty()) 
	{
		cur = q.front();//一定得是cur，因为temp，是更新用到，要使用temp的话，会保留以前的结果，使之结果不准确。
		q.pop();
		for(int i = 0; i < 9; i++) //搜索八个方向
		{
			temp.x = cur.x + dx[i]; 
			temp.y = cur.y + dy[i];
			temp.step = cur.step + 1;
			//判断没有越界，上一步没碰见大石头，这一步没碰见大石头 
			if(check(temp.x,temp.y) && a[temp.x - temp.step][temp.y] != 'S' && a[temp.x - temp.step + 1][temp.y] != 'S')
			{
				//看是否走完了
				if(temp.step > 8 || a[temp.x ][temp.y ] == 'A') 
				{
					flag = 1;
					return;
				}
				q.push(temp);
			} 
		} 
	}
    return ;
}

int main()
{
	int t = 0;
	cin >> t;
	int k = 1;
	while (t--)
	{
		for(int i = 0;i < 8; i++)
		{
			getchar(); //匹配回车字符。
			scanf("%s",a[i]);
		}
		flag = 0;
		bfs();
		printf("Case #%d: ",k);
		if(flag) cout << "Yes" << endl;
		else cout << "No" << endl;
		k++;
	}
	return 0;
}
```

马的遍历(https://www.luogu.com.cn/problem/P1443)

```c
//bfs要知道是否走过没走过，这个信息除了是否出界以外，是最重要的判断因素
#include <iostream>
#include <cstdio>
#include <map>
#include <string>
#include <queue> 
#include <cstring>
#include <algorithm>
#include<cmath>
using namespace std;
struct node {
	int x;
	int y;
}cur, temp;

int n = 0, m = 0;
int a[410][410]; //判断每步行走的步数
bool b[410][410];//判断是否走过
int dx[] = {0, -2, -2, -1, -1, 1,1,2,2 };
int dy[] = {0,1, -1, 2, -2,2, -2,1,-1 };//遍历八个方向，马走日

void bfs(int x, int y)
{
	cur.x = x;
	cur.y = y;
	queue<node>q;
	q.push(cur);
	while (!q.empty())
	{
		cur = q.front();
		q.pop();
		for (int i = 1; i <= 8; i++)
		{
			temp.x = cur.x + dx[i];
			temp.y = cur.y + dy[i];
			if (temp.x < 1 || temp.x > n || temp.y < 1 || temp.y > m || b[temp.x][temp.y]) continue; //出界或走过就不走
			b[temp.x][temp.y] = true;
			a[temp.x][temp.y] = a[cur.x][cur.y] + 1;
			q.push(temp);
		}
	}
	return;
}
int main()
{
	int row = 0, col = 0;
	cin >> n >> m >> row >> col;
	memset(a, -1, sizeof(a));//表示每步棋先都为-1
	memset(b, false, sizeof(b));
	a[row][col] = 0, b[row][col] = true;
	bfs(row, col);
	for (int i = 1; i <= n; i++)
	{
		for (int j = 1; j <= m; j++)
		{
			if (j != m) printf("%-5d",a[i][j]);
			else printf("%-5d\n",a[i][j]);
		}
	}
	return 0;
}
```

奇怪的楼梯(https://www.luogu.com.cn/problem/P1135)

```c
//此题解决方式依然是，判断该加上/减去ki的值的楼是否走过并且加/减上ki的值的楼高度是否超过最高值/最低值
#include <iostream>
#include <cstdio>
#include <map>
#include <string>
#include <queue> 
#include <cstring>
#include <algorithm>
#include<cmath>
using namespace std;
//该题的问题简化了，只有两个方向
struct node {
	int x;//第几楼层
	int step;//到第几楼所需要的步数
}cur;
queue<node>q;
int n = 0, a = 0, b = 0;
int k_i[1000];
bool visit[1000] = { false };
//如果加上/减去k_i的值超出楼层范围，并且该楼层走过了，可以不用选则
int main()
{
	cin >> n >> a >> b;
	for (int i = 1; i <= n; i++)
	{
		scanf("%d", &k_i[i]);
	}
	cur.x = a, cur.step = 0;
	q.push(cur);
	while (!q.empty())
	{
		node temp;
		cur = q.front();
		q.pop();
		if (cur.x == b) break;
		if (cur.x + k_i[cur.x] <= n && visit[cur.x + k_i[cur.x]] == false)
		{
			temp.x = cur.x + k_i[cur.x];
			temp.step = cur.step + 1;
			q.push(temp);
			visit[temp.x] = true;
		}
		if (cur.x - k_i[cur.x] > 0 && visit[cur.x - k_i[cur.x]] == false)
		{
			temp.x = cur.x - k_i[cur.x];
			temp.step = cur.step + 1;
			q.push(temp);
			visit[temp.x] = 1;
		}
	}
	if (cur.x == b) printf("%d", cur.step);
	else printf("-1");
	return 0;
	//此题还有个小心之处，当提前达到目标楼，需要提前终止
}
```

颜色填充

```c
//洪水填充法
#include <iostream>
#include <cstdio>
#include <map>
#include <string>
#include <queue> 
#include <cstring>
#include <algorithm>
#include<cmath>
using namespace std;
int n = 0, num[35][35] = { 0 };
int visit[35][35] = { 0 };
int dx[] = { 0,0,1,-1 };
int dy[] = { 1,-1,0,0 };
void bfs(int x, int y)
{
	visit[x][y] = 1;
	num[x][y] = 3;
	queue < pair<int, int> > q;
	q.push({ x,y });
	while (!q.empty())
	{
		int xx = q.front().first, yy = q.front().second;
		q.pop();
		for (int i = 0; i < 4; i++)
		{
			int xxx = xx + dx[i], yyy = yy + dy[i];
			if (xxx < 0 || xxx > n + 1 || yyy < 0 || yyy > n + 1 || visit[xxx][yyy] == 1) continue;
			if (num[xxx][yyy] == 1) continue;
			num[xxx][yyy] = 3;
			q.push({ xxx,yyy });
			visit[xxx][yyy] = 1;
		}
	}
	return;
}
int main()
{
	cin >> n;
	for (int i = 1; i <= n; i++)
	{
		for (int j = 1; j <= n; j++)
		{
			cin >> num[i][j];
		}
	}
	bfs(0, 0);
	for (int i = 1; i <= n; i++)
	{
		for (int j = 1; j <= n; j++)
		{
			if (num[i][j] == 3) printf("0 ");
			if (num[i][j] == 1) printf("1 ");
			if (num[i][j] == 0) printf("2 ");
		}
		cout << endl;
	}
	return 0;
}
```

八数码(http://codeup.cn/problem.php?cid=100000609&pid=2)

```c
#include <cstring>
#include <cstdio>
#include <iostream>
#include <queue>
#include <algorithm>
#include <string>
#include <unordered_map>

using namespace std;

int dx[] = { 0,0,-1,1 }; //方向数组
int dy[] = { 1,-1, 0,0 };

int bfs(string start,string end)
{
	unordered_map<string, int>d;
	queue < string >q;
	
	q.push(start);
	d[start] = 0;
	int distance = 0;
	while (!q.empty())
	{
		string cur = q.front();
		q.pop();
		distance = d[cur];

		if (cur == end) {
			return distance;
		}

		int index = cur.find("0");
		int x = index / 3; //一维转二维
		int y = index % 3;
		for (int i = 0; i < 4; i++) //搜索四个方向
		{
			int xx = x + dx[i];
			int yy = y + dy[i];
			if (xx >= 0 && xx < 3 && yy >= 0 && yy < 3)
			{
				swap(cur[index], cur[xx * 3 + yy]); //二维转一维
				if (d.count(cur) == 0) //即查找cur是否在d中存在，不存在返回0
				{
					d[cur] = distance + 1;
					q.push(cur);
				}
				swap(cur[index], cur[xx * 3 + yy]); //还原
			}
		}
	}
	return -1;
}

int main()
{
	string start = "";
	string end = "";
	char c;
	for (int i = 0; i < 3; i++)
	{
		for (int j = 0; j < 3; j++)
		{
			cin >> c;
			start += c;
		}
	}
	for (int i = 0; i < 3; i++)
	{
		for (int j = 0; j < 3; j++)
		{
			cin >> c;
			end += c;
		}
	}
	cout << bfs(start, end) + 1; //加一是因为最后的变换也算
	return 0;
}


```



### 十六、树

#### 1、二叉树的动态实现（链式）

```c
//这里适合一个函数一个函数去复习
#include <iostream>
#include <cstdio>
#include <string>
#include <vector>
#include <queue>
#include <algorithm>
using namespace std;

struct BTreeNode
{
	int date = 0;
	BTreeNode *left;
	BTreeNode *right;
};

class BTree {
public:
	BTree() {

	}
	//传参数的时候要注意，二叉树的结点时一个指针，若要改变二叉树结点的值，需要二级指针
	void create(BTreeNode* &Node)// 创建二叉树
	{
		int date;
		cin >> date;
		if ( date )
		{
			Node = new BTreeNode;
			Node->date = date;
			create(Node->left);
			create(Node->right);
		}
		else {
			Node = NULL;
		}
		
    }
	//按层创建二叉树
	void levelBTreeNode(BTreeNode * & Node)  //改变内容用二级指针
	{
		queue <BTreeNode*> q;
		int date = 0;
		cin >> date;
		if (date)
		{
			Node = new BTreeNode;
			Node->date = date;
			q.push(Node);
		}
		else {
			Node = NULL;
			return;
		}
		while (!q.empty())
		{
			BTreeNode *node = q.front();
			q.pop();
			//输入左子树
			cin >> date;
			if (date)
			{
				node -> left = new BTreeNode;
				node->left->date = date;
				q.push(node -> left);
			}
			else {
				node->left = NULL;
			}
			//输入右子树
			cin >> date;
			if (date)
			{
				node->right = new BTreeNode;
				node->right->date = date;
				q.push(node->right);
			}
			else {
				node->right = NULL;
			}
		}
	}
	//清空二叉树
	void clear( BTreeNode* & Node )
	{
		BTreeNode *p = Node;
		if (p != NULL)
		{
			clear(p->left);
			clear(p->right);
			delete p;
		}

	}
	//前序遍历（根左右）
	void preorderTree(BTreeNode*  Node)
	{
		if (Node != NULL)
		{
			cout << Node->date << ",";
			preorderTree(Node->left);
			preorderTree(Node->right);
		}
	}
	//中序遍历（左根右）
	void inorderTree(BTreeNode*  Node)
	{
		if (Node != NULL)
		{
			inorderTree(Node->left);
			cout << Node->date << ",";
			inorderTree(Node->right);
		}
	}
	//后序遍历
	void postorderTree(BTreeNode* Node)
	{
		if (Node != NULL)

		{
			postorderTree(Node->left);
			postorderTree(Node->right);
			cout << Node->date << ",";
		}
	}
	//图层遍历
	void levelTree(BTreeNode* Node)
	{
		queue <BTreeNode*> que;
		que.push(Node)
		while(!que.empty())
		{
			BTreeNode* node = que.front();
			cout << node->date << ",";
			que.pop();
			if (node->left != NULL) que.push(node->left);
			if (node->right != NULL) que.push(node->right);
		}

	}
	//二叉树深度
	int lenthofTree(BTreeNode* Node)
	{
		if (Node != NULL)
		{
			return max(lenthofTree(Node->left), lenthofTree(Node->right)) + 1;
		}
		else {
			return 0;
		}
	}
	//树的结点个数
	int getNodeNum(BTreeNode * Node)
	{
		if (Node != NULL)
		{
			return 1 + getNodeNum(Node->left) + getNodeNum(Node->right);
		}
		else {
			return 0;
		}
	}
	//叶节点的返回个数
	int getLeafNum(BTreeNode * Node)
	{
		if (Node == NULL) return 0;
		else {
			if (Node->left == NULL && Node->right == NULL)
			{
				return 1;
			}
			else {
				return getLeafNum(Node->left) + getLeafNum(Node->right);
			}
		}
	}

};

int main ()
{
	BTree tree;
	BTreeNode * root;
	//tree.create(root);
	tree.levelBTreeNode(root);//利用图层遍历创造二叉树
	cout << "pre:";
	tree.preorderTree(root);
	cout << endl;
	cout << "in:";
	tree.inorderTree(root);;
	cout << endl;
	cout << "post:";
	tree.postorderTree(root);
	cout << endl;
	cout << "level:";
	tree.levelTree(root);
	cout << endl;
	cout << tree.lenthofTree(root) << tree.getNodeNum(root) << tree.getLeafNum(root) << endl;
	tree.clear(root);
   return 0; 
} 
```

实例

```c
/*问题描述；例如如下的先序遍历字符串：
ABC##DE#G##F###
其中“#”表示的是空格，空格字符代表空树。建立起此二叉树以后，再对二叉树进行中序遍历，输出遍历结果。*/
#include<iostream>
#include<cmath>
#include<algorithm>
#include<string>
#include<cstdio>
#include<queue>
using namespace std;
struct node {
	char data;
	node* left;
	node* right;
};
string pre;
int i;
node* newbtree() //先序遍历的方式，转换为链表存储
{
	if (pre[i] == '#')
	{
		i++;
		return NULL;
	}
	node* Node = new node;
	Node->data = pre[i];//先序遍历结构
	i++;
	Node->left = newbtree();
	Node->right = newbtree();
	return Node;
}

void inorder(node* root)
{
	if (root != NULL)
	{
		inorder(root->left);
		cout << root->data << " ";
		inorder(root->right);
	}
}
int main()
{
	while (cin >> pre)
	{
		getchar();
		i = 0;
		node* root = newbtree();
		inorder(root);
		cout << endl;
	}
	return 0;
}
```



#### 2、通过给定两个序列，重建二叉树

```c
//给定前序和中序遍历，构建树，并输出其层序遍历
#include <iostream>
#include <algorithm>
#include <string>
#include <cstdio>
#include <queue>
using namespace std;
const int maxn = 50;
struct node{
	int data;
	node *left;
	node *right;
};

int pre[maxn], in[maxn],post[maxn]; //先序，中序，后序 
int n ; //结点个数
node *create(int preL, int preR, int inL, int inR)
{
	if(preL > preR) return NULL;
	node *root = new node; //建立一个新节点，来存放当前二叉树根节点 
	root -> data = pre[preL]; //新节点的数据域为根节点的值 
	int k; //用来找中序遍历的根节点的下标 ,主要是针对中序遍历的变量 
	for(k = inL; k <= inR; k++)
	{
		if(pre[preL] == in[k])
		{
			break;
		} 
	}
	int numleft = k - inL; //左子树节点的数量；这个变量主要是针对前序遍历
	//左子树先序区间为[preL + 1, pre + numleft]; 中序左子树区间[inL, k - 1];
	//返回左子树根节点地址，赋给root的左指针
	root->left = create(preL + 1, preL + numleft,inL, k - 1);
	//右子树区间先序[preL + 1 + nulleft,preR],中序区间[k + 1,inR];
	root->right = create(preL + 1 + numleft,preR, k + 1, inR);
	return root; 
} 

void bfs(node *Node)
{
	queue<node*>q;
	q.push(Node);
	while(!q.empty())
	{
		node *cur = q.front();
		q.pop();
		printf("%d ",cur->data );
		if(cur->left != NULL) q.push(cur->left );
		if(cur->right != NULL) q.push(cur->right );
	}
}
int main ()
{
	scanf("%d",&n);
	for(int i = 0; i < n; i++)
	{
		scanf("%d",&pre[i]);
	} 
	for(int i = 0; i < n; i++)
	{
		scanf("%d",&in[i]);
	}
	node *root = create(0,n - 1, 0, n - 1); //建树
	bfs(root); //层序遍历 
	return 0;
} 
//类似题，输入数据不一样
//当输入输出只有字符串来充当先序，后序遍历时，可以有个简便方法
#include<iostream>
#include<cmath>
#include<algorithm>
#include<string>
#include<cstdio>
#include<queue>
#include<vector>
using namespace std;
void postorder(string preorder,string inorder)//实质都可以写成dfs
{
	if (inorder.size() != 0) {
		char op = preorder[0];
		int x = inorder.find(op);
		postorder(preorder.substr(1, x), inorder.substr(0, x));
		postorder(preorder.substr(x + 1), inorder.substr(x + 1, inorder.size() - 1 - x));
		cout << op;
	}
}
int main()
{
	string preorder, inorder;
	cin >> inorder >> preorder;
	postorder(preorder, inorder);
	return 0;
}
```

给定前序遍历和后续遍历，来确定中序遍历有多少种

```c
//先序遍历中，如果A只有一个子树B，那么在先序遍历中A一定在B前，在后序遍历中B一定在A前
//前序遍历AB,后续遍历BA,那么中序遍历为（AB,BA）;每个这类节点有两种中序遍历（及儿子在左，儿子在右）根据乘法原理中序遍历数为 2^节点个数 种
//所以将题目转化成找 只有一个儿子的节点个数。
//找这种情况AB
//           BA 
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <queue> 
#include <vector>
#include <string>
using namespace std;

int main ()
{
	string a,b;
	cin >> a ;
	getchar();
	cin >> b;
	int ans = 0;
	for(int i = 0; i < a.size(); i++)
	{
		for(int j = 1; j < b.size(); j++)
		{
			if(a[i] == b[j] && a[i + 1] == b[j - 1]){
				ans++;
			}
		}
    }
    printf("%d",1<<ans);
	return 0;
}
```



#### 3、二叉树的静态实现

```c++
//大致模板
struct node{
    int data;
    int left;
    int right;
}Node[maxn];

//节点的生成
int index = 0;
int newNode(int v)
{
    Node[index].data = v;
    node[index].left = -1; //表示空
    node[index].right = -1;
    return index++;
}
//二叉树的查找
void search(int root,int x, int newdata){
    if(root == -1) return;
    if(Node[root].data == x) {
        Node[root].data = newdata;
    }
    search(Node[root].left, x,newdata);
    search(Node[root].right,x,newdata);
}
//插入
void insert(int &root, int x){
    if(root == -1){ //空树，没有插入可言，插入后就一个结点
        root = newNode(x);
        return;
    }
    if(有二叉树的性质，因插入在左子树){
        insert(Node[root].left,x);
    }else{
        insert(Node[root].right,x);
    }
}
//二叉树的建立
int create(int data[],int n)
{
    int root = -1;//建立根节点
    for(int i = 0; i < n; i++)
    {
        insert(root,data[i]); //将data[0]-data[n - 1] 插入二叉树中
    }
    return root;
}
//先序遍历
void preorder(int root)
{
    if(root == -1) return;
    printf("%d ",Node[root].data);
    preorder(Node[root].left);
    preorder(Node[root].right);
}
void inorder(int root) //中序遍历
{
    if(root == -1) return;
    inorder(Node[root].left);
    printf("%d ",Node[root].data);
    inorder(Node[root].right);
}
void postorder(int root) //后序遍历
{
    if(root == -1) return;
    postorder(Node[root].left);
    postorder(Node[root].right);
    printf("%d ",Node[root].data);
}
//层序遍历
void levelorder(int root){
    queue<int>q;
    q.push(root);
    while(!q.empty()){
        int cur = q.front();
        q.pop();
        printf("%d ",Node[cur].data);
        if(Node[cur].left != -1) q.push(Node[cur].left);
        if(Node[cur].right != -1) q.push(Node[cur].right);
    }
}
```

实例；

```c
#include<iostream>
#include<cmath>
#include<algorithm>
#include<string>
#include<cstdio>
#include<queue>
#include<vector>
using namespace std;
struct node {
	char left;
	char right;
}tree[200]; //每个结点的（孩子结点）
int n = 0;
bool st[1000] = { false };//目的是，把孩子结点做标记，以此来找到父亲节点
char h[100];//父亲节点
void preorder(char op)
{
	if (op == '*') return;
	cout << op;
	preorder(tree[op].left);
	preorder(tree[op].right);
}
int main()
{
	cin >> n;
	for (int i = 0; i < n; i++)
	{
		cin >> h[i];
		cin >> tree[h[i]].left >> tree[h[i]].right;
		st[tree[h[i]].left] = st[tree[h[i]].right] = true;//让孩子结点都做标记，从而找到根节点
	}
	for (int i = 0; i < n; i++)
	{
		if (st[h[i]] == false)
		{
			preorder(h[i]);//从而找到根节点
		}
	}
	return 0;
}
```

> 完全二叉树：通过给出后序遍历还输出层次遍历；
>
> 方法：根据性质，从1出发，递归i*2, i * 2 + 1,，最下面来用一个数组从当前变量存储即可，cin >> tree[i];

```c

```



二叉树深度（几层）(https://www.luogu.com.cn/problem/P4913)

```c
#include<iostream>
#include<cmath>
#include<algorithm>
#include<string>
#include<cstdio>
#include<queue>
#include<vector>
using namespace std;
int n = 0;
const int maxn = 1e6 + 10;
struct node {
	int left;
	int right;
}tree[maxn];
int Max = -1;
void dfs(int root, int dept)
{
	if (root == 0) return;
	Max = max(Max, dept);
	dfs(tree[root].left, dept + 1);
	dfs(tree[root].right, dept + 1);
}
int main()
{
	cin >> n;
	for (int i = 1; i <= n; i++)
	{
		cin >> tree[i].left >> tree[i].right;
	}
	dfs(1, 1);
	cout << Max;
	return 0;
}
```



#### 4、树的遍历

```c
//大致
struct node{
    typename data;
    vector<int> child;
}Node[manx];

int index = 0;
int NewNode(int v)
{
    Node[index].data = v;
    Node[indnex].child.clear(); //清空子节点
}
//先根遍历
void preorder(int root)
{
    printf("%d ",Node[root].data);//访问当前节点
    for(int i = 0; i < Node[root].child.size();i++)
    {
        preorder(Node[root].child[i]);//递归访问root的所有子结点
    }
}
//层序遍历
void levelorder(int root)
{
    queue<int>q;
    q.push(root);
    while(!q.empty()){
        int cur = q.front();
        q.pop();
        printf("%d ",Node[cur].data);
        for(int i = 0; i < Node[cur].child.size(); i++){
            q.push(Node[cur].child[i]);
        }
    }
}
//如果对层号进行便历的话
struct node{
    typename data;
    int layer;
    vector<int>child;
}Node[maxn];
void levelorder(int root)
{
    queue<int>q;
    q.push(root);
    Node[root].layer = 0;//记录根节点层号为0
    while(!q.empty()){
        int cur = q.front();
        q.pop();
        printf("%d ",Node[cur].data);
        for(int i = 0; i < Node[cur].child.size();i++){
            int temp = Node[cur].child[i];
            Node[temp].layer = Node[cur].layer + 1;
            q.push(temp);
        }
    }
}
```

实例；1,

```c
//输入有多组数据。每组输入一个n(1<=n<=1000)，然后将树中的这n个节点依次输入，再输入一个d代表深度。
//输出；输出该树中第d层得所有节点，节点间用空格隔开，最后一个节点后没有空格。（要知道该数是一个完全二叉树）,没有输出EMPTY
#include<iostream>
#include<cmath>
#include<algorithm>
#include<string>
#include<cstdio>
#include<queue>
#include<vector>
using namespace std;

int main()
{
	int n;
	while (cin >> n && n != 0) {
		//cout << "n = " << n;
		vector <int> a(n + 1);
		for (int i = 1; i <= n; i++)
		{
			cin >> a[i];
		}
		int d = 0;
		cin >> d;//求该层的所有结点
		int left = pow(2, d - 1); //这样算出来时完全二叉树该层数的最左端
		int right = pow(2, d) - 1;//是要求该层数的最右端
		if (n < left) {
			printf("EMPTY\n");
			continue;
		}
		if (n < right) {
			right = n; //制定该层的有效数据区域
		}
		for (int i = left; i <= right; i++)
		{
			printf("%d", a[i]);
			if (i != right) printf(" ");
		}
		printf("\n");
	}
	return 0;
}
```

2、树的遍历（静态写法）[问题 B: 树的高度](http://codeup.cn/problem.php?cid=100000612&pid=1)

```c
//树的静态写法，同时保存结点编号最大的值，层序遍历更新每个结点的层次，最后输出最大编号的结点的层次
#include<iostream>
#include<cmath>
#include<algorithm>
#include<string>
#include<cstdio>
#include<queue>
#include<vector>
using namespace std;
struct node {
	int data;
	int layer;
	vector<int>child;
}Node[1000]; //每个结点的（数据，层次数，孩子结点）
int maxn = 1;//表示结点数据的最大编号
int n = 0; //节点数

void bfs()
{
	queue<int>q;
	q.push(1);//存入的是结点数据（第一层结点数据是1）
	Node[1].layer = 1;
	while (!q.empty()) {
		int top = q.front();
		q.pop();
		for (int i = 0; i < (int)Node[top].child.size(); i++) //遍历该结点的孩子
		{
			int child = Node[top].child[i];
			Node[child].layer = Node[top].layer + 1;
			q.push(child);//孩子节点入队
		}
	}
	printf("%d\n", Node[maxn].layer);
}
int main()
{
	while (cin >> n) {
		int a, b;
		maxn = 1;
		for (int i = 0; i < n - 1; i++)
		{
			cin >> a >> b;
			maxn = max(a, b);
			Node[a].child.push_back(b); //整个循环遍历完之后，可以去确定每个结点有几个孩子结点
		}
		bfs();
	}
	return 0;
}
```

#### 5、二叉查找树（BST）

> 性质；左子树小于根节点，右子树大于根节点。

```c
//大致模板
struct node{
	int data;
	node* left;
	node* right;
}; 
int n = 0;//节点数
int data[100]; 
//新建结点 
node* newNode(int v)
{
	node* Node = new node;
	Node->data = v;
	Node->left = Node->right = NULL;
	return Node;
}
//二叉树的查找 
void search(node *root,int x) 
{
	if(root == NULL) {
		printf("空树，查找失败\n");
		return ; 
	}
	if(x == root->data){//查找成功 
		printf("查找成功：%d\n",root->data );
	} else if(x < root->data) { //再root的左子树上 
		search(root->left,x); 
	} else if(x > root->data) {
		search(root->right,x);
	}
	
}

//二叉查找树的插入
void insert(node* & root,int x)
{
	if(root == NULL) //空树，查找失败，即插入位置
	{
		printf("插入成功"); 
		root = newNode(x);
		return;
	} 
	if(x == root->data ) //查找成功，结点存在，直接返回
	{
		return ; 
	} else if( x < root->data ) {
		insert(root->left,x);
	} else {
		insert(root->right,x);
	}
} 

//二叉查找树的建立
node* create(int data[], int n)
{
	node* root = NULL; //新建根节点
	for(int i = 0; i < n; i++)
	{
		insert(root,data[i]);//将data[0]~data[n-1]插入到root中 
	} 
	return root; 
} 
//寻找以root为根节点的树的最大权值结点 
node* findmax(node* root)
{
	while(root->right != NULL) //不断往右，直到没孩子
	{
		root = root->right;
	} 
	return root;
}
node* findmin(node*root) //找最小权值结点
{
	while(root->left != NULL)
	{
		root = root->left;
	}
	return root;
} 

void deleteNode(node* &root, int x)
{
	if(root == NULL) {
		printf("没有要删除的结点:\n");
		return;
	}
	if(root->data == x) {//找到删除的节点了 
		if(root->left == NULL && root->right == NULL)//叶子结点直接删除
		{
			root = NULL;
		} else if(root->left != NULL) //左子树不为空，找左子树最大权值结点
		{
			node* pre = findmax(root->left );
			root->data = pre->data;
			deleteNode(root->left,pre->data);
		} else if(root->right != NULL) {
			node* next = findmin(root->right );
			root->data = next->data;
			deleteNode(root->right ,next->data);
		}
	} else if(root->data > x) {
		deleteNode(root->left,x);
	} else if(root->data < x) {
		deleteNode(root->right,x);
	}
}
```

实例；

```c
//二叉排序树的建立，以及遍历
#include <iostream>
#include <cmath>
#include <algorithm>
#include <string>
#include <cstdio>
#include <queue>
#include <vector>
using namespace std;
struct node {
	int data;
	node* left;
	node* right;
};
int n = 0;
node* create(node*& root, int x)
{
	if (root == NULL)
	{
		root = new node;
		root->data = x;
		root->left = root->right = NULL;
		return root;
	}
	else if (x < root->data) {//往左子树插入
		root->left = create(root->left, x);
	}
	else if (x > root->data) {//往右子树插入 //这里不能直接右else,因为还有x == root->data
		root->right = create(root->right, x);
	}
	return root;
}
void preorder(node* root)
{
	if (root != NULL)
	{
		cout << root->data << " ";
		preorder(root->left);
		preorder(root->right);
	}
}
void inorder(node* root)
{
	if (root != NULL)
	{
		inorder(root->left);
		cout << root->data << " ";
		inorder(root->right);
	}
}
void postorder(node* root)
{
	if (root != NULL)
	{
		postorder(root->left);
		postorder(root->right);
		cout << root->data << " ";
	}
}
int main()
{
	while (cin >> n)
	{
		node* root = NULL;
		int ans;
		for (int i = 0; i < n; i++)
		{
			cin >> ans;
			root = create(root, ans);
		}
		preorder(root);
		cout << endl;
		inorder(root);
		cout << endl;
		postorder(root);
		cout << endl;
	}
	return 0;
}
```

```c
//判断两个序列是否为同一二叉搜索树
//二叉排序树的建立，以及多个二叉搜索树的比较是否为同一二叉树
#include <iostream>
#include <cmath>
#include <algorithm>
#include <cstring>
#include <cstdio>
#include <queue>
#include <vector>
using namespace std;
struct node {
	int data;
	node* left;
	node* right;
};
int n = 0;
int flag = 0;
node* create(node* root, int x)
{
	if (root == NULL)
	{
		root = new node;
		root->data = x;
		root->left = root->right = NULL;
		return root;
	}
	else if (x < root->data) {
		root->left = create(root->left, x);
	}
	else if (x > root->data) {
		root->right = create(root->right, x);
	}
	return root;
}

void compare(node* root1, node* root2)
{
	if (root1 == NULL && root2 != NULL) {
		flag = 1;
		return;
	}
	else if (root1 != NULL && root2 == NULL) {
		flag = 1;
		return;
	}
	else if (root1 != NULL && root2 != NULL) {
		if (root1->data != root2->data) {
			flag = 1;
			return;
		}
		else {
			compare(root1->left, root2->left);
			compare(root1->right, root2->right);
		}
	}
}
int main()
{
	while (cin >> n && n != 0)
	{
		char a[12], b[12];
		int ans = 0;
		node* root1 = NULL;
		cin >> a;
		getchar();
		ans = strlen(a);
		for (int i = 0; i < ans; i++)
		{
			root1 = create(root1, a[i] - '0');
		}
		for (int i = 0; i < n; i++)
		{
			flag = 0;
			node* root2 = NULL;
			cin >> b;
			getchar();
			int count = strlen(b);
			for (int j = 0; j < count; j++)
			{
				root2 = create(root2, b[j] - '0');
			}
			compare(root1, root2);
			if (flag == 0) printf("YES\n");
			else printf("NO\n");
		}
	}
	return 0;
}
```

#### 6、平衡二叉树（AVL）

```c
//大致模板
struct node{
	int data;//结点权值
	int height;//当前子树高度 
	node* left;
	node* right;
}; 
int n = 0;//节点数
int data[100]; 

int getheight(node* root) //获取当前接结点root的高度
{
	if(root == NULL) return 0; //空树，结点为0
	return root->height; 
} 

//计算结点root的平衡因子
int getbalancefactor(node* root)
{
	//左子树高度减右子树高度
	return getheight(root->left ) - getheight(root->right ); 
} 
//更新结点root的height
void updataheight(node* root)
{
	//max(左孩子的heiht,右孩子的height)+ 1;
	root->height = max(getheight(root->left),getheight(root->right) ) + 1; 
} 
//平衡二叉树的查找(在二叉查找树的基础上)
void search(node* root, int x)
{
	if(root == NULL) { //空树 
		printf("查找失败\n" );
		return; 
	} 
	if(x < root->data ) {
		search(root->left,x);
	} else if(x > root->data ){
		search(root->right,x);
	} else {
		printf("查找到了%d\n",root->data );
	}
} 
//左旋
void L(node* &root)
{
	node* temp = root->right; //第一步 
	root->right = temp->left;//第二步 
	temp->left = root;
	updataheight(root);//更新结点高度 
	updataheight(temp);
	root = temp;//第三步 
} 
//右旋
void R(node* &root)
{
	node* temp = root->left;
	root->left = temp->right;
	temp->right = root;
	updataheight(temp);
	updataheight(root);
	root = temp;
} 
//平衡二叉树的插入
void insert(node* &root, int x)
{
	if(root == NULL) //要插入的位置
	{
		root = new node;
		root->data = x;
		root->height = 1;
		root->left = root->right = NULL;
		return ;
	} 
	if(x < root->data) {
		insert(root->left,x);
		updataheight(root); //更新树高度
		if(getbalancefactor(root) == 2) {
			if(getbalancefactor(root->left) == 1) //LL型
			{
				R(root);//左旋一下就行 
			} else if(getbalancefactor(root->left) == -1) //LR型
			{
				L(root->left);
				R(root);
			} 
		} else {
			insert(root->right,x);
			updataheight(root);
			if(getbalancefactor(root) == -2) {
				if(getbalancefactor(root->right) == -1) //RR型
				{
					L(root);
				} else if(getbalancefactor(root->right) == 1) //RL型
				{
					R(root->right );
					L(root);
				} 
			}
		}
	}
} 
//AVL树的建立
node* create(int data[], int n)
{
	node* root = NULL;
	for(int i = 0; i < n; i++)
	{
		insert(root,data[i]);
	} 
	return root;
} 
```

插入操作完整代码（实例）

```c++
//插入，查找代码
#include <iostream>
#include <cstdio>
#include <algorithm>
using namespace std;
struct node{
	int data;
	int height;
	node* left;
	node* right;
	
};
int getheight(node* root) //当前结点高度
{
	if(root == NULL) return 0;
	return root->height;
} 
//计算平衡因子
int getbalancefactor(node* root)
{
	//左子树-右子树
	return (getheight(root->left) - getheight(root->right)); 
} 
//更新结点root的height
void updataheight(node* root)
{
	root->height = max(getheight(root->left ),getheight(root->right )) + 1;
} 
void L(node* &root) //左旋
{
	node* temp = root->right;
	root->right = temp->left;
	temp->left = root;
	updataheight(root);
	updataheight(temp);
	root = temp; 
} 
void R(node* &root) //右旋
{
	node* temp = root->left;
	root->left = temp->right;
	temp->right = root;
	updataheight(root);
	updataheight(temp);
	root = temp;
} 
void insert(node* &root,int x)
{
	if(root == NULL)
	{
		//printf("找到插入点\n");
		root = new node;
		root->data = x;
		root->height = 1;
		root->left = root->right = NULL;
		//cout << "root->data = " << root->data << endl;
		return;
	}
	if(x < root->data ){
		insert(root->left,x);
		updataheight(root);
		if(getbalancefactor(root) == 2) {
			if(getbalancefactor(root->left ) == 1){ //LL
				R(root);
			} else if(getbalancefactor(root->left) == -1){//LR
				L(root->left );
				R(root);
			}
		} 
	}else if(x > root->data){
		insert(root->right,x);
		updataheight(root);
		if(getbalancefactor(root) == -2) {
			if((getbalancefactor(root->right) == -1)) {//RR
				L(root);
			}else if(getbalancefactor(root->right ) == 1){ //RL
				R(root->right );
				L(root);
			}
		}
	}
}

int search(node* root, int x)
{
	if(root == NULL) return 0;
	if(root->data == x)
	{
		return 1;
	} else if(x < root->data){
		search(root->left,x);
	}else if(x > root->data ){
		search(root->right,x);
	}
}
//AVL树的建立 
node* create(int data[], int n)
{
	node* root = NULL;
	for(int i = 0; i < n; i++)
	{
		insert(root,data[i]);
	} 
	return root;
}
int main ()
{
	int n = 0, k = 0;
    while(cin >> n >> k){
    	int a[n + 2];
    	for(int i = 0; i < n; i++)
    	{
    		scanf("%d",&a[i]);
		}
		node* root = create(a,n);
		int m;
		for(int i = 0; i < k; i++)
		{
			scanf("%d",&m);
			printf("%d",search(root,m));
			if(i != k-1) cout << " ";
			else cout << endl;
		}
		
	}
	return 0;
}
```

#### 7、并查集&(并查集的压缩路径)

```c++
//大致模板
//初始化，让元素都归为一个集合
void init(int n) {
	for(int i = 1; i <= n; i++)
	{
		father[i] = i;
	}
} 
//查找(函数返回元素x所在的根节点)
int findfather(int x)
{
	while(x != father[x]){//如果不是根节点 
		x = father[x];//获取自己的根节点 
	}
	return x; 
} 

//合并（将两个集合合并成一个集合）
void Union(int a, int b)
{
	int fa = findfather(a);//查找a的根节点 
	int fb = findfather(b);//b的根节点 
	if(fa != fb){ //不是同一个集合 
		father[fa] = fb;//合并他们 
	}
} 
//查找的的压缩路径，既让一个集合都是叶子结点，且只有一个根节点
//这样减少了查找到时间 
int Findfather(int x)
{
	int z = x;
	while(x != father[x]){//找到根节点 
		x = father[x];
	}
	//x存放的是根节点，下面把路径上所有结点的father都改成根节点 
	while(z != father[z]){
		int a = z; //因为z要被fathe的根节点覆盖，所以先保存
		z = father[z];
		father[a] = x;//原先父亲节点改为根节点 
	} 
	return x; 
}
```

算法实现

```c++
//输入几组谁跟谁是好友，再判断有几个集合 
#include <iostream>
#include <cstdio>
#include <algorithm>
using namespace std;
const int maxn = 110;
int father[maxn];
bool isroot[maxn] = {false}; //记录每个结点是否可以作为根节点 
int findfather(int x)
{
	int z = x;
	while(x != father[x])
	{
		x = father[x];
	}
	//压缩路径
	while(z != father[z])
	{
		int a = z;
		z = father[z];
		father[a] = x;
	} 
	return x;
}
void Union(int a, int b)
{
	int fa = findfather(a);
	int fb = findfather(b);
	if(fa != fb){
		father[fa] = fb;
	}
} 

//初始化
void init(int n)
{
	for(int i = 1; i <= n;i ++)
	{
		father[i] = i;
		isroot[i] = false;
	}
} 

int main ()
{
	int m = 0, n = 0;
	cin >> n >> m;
	init(n); //初始化数据
	int a,b;
	for(int i = 0; i < m; i++)
	{
		cin >> a >> b;
		Union(a,b);
	} 
	for(int i = 1; i <= n; i++)
	{
		isroot[findfather(i)] = true; //有几个根节点就会有几个集合 
	}
	int count = 0;
	for(int i = 1; i <= n; i++)
	{
		if(isroot[i]) count++;
	} 
	cout << count;
	return 0;
}
```

#### 8、堆排序

略

#### 9、哈夫曼树和编码

```c
//哈夫曼树，实质是最优二叉树，把所有叶子节点合并成一棵树，且带权路径长度之和最小的过程 （结点的值与权值的乘积之和）
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <queue> 
#include <vector>
using namespace std;
//小顶堆的优先序列 
priority_queue<long long,vector<int>, greater<long long> >q;

int main ()
{
	int n;
	long long temp, x,y,ans = 0;
	scanf("%d",&n);
	for(int i = 0; i < n; i++)
	{
		scanf("%lld",&temp);
		q.push(temp);//入队 
	} 
	while(q.empty() > 1) //只要优先队列中有两个元素 
	{
		x = q.top();
		q.pop();
		y = q.top();
		q.pop();
		q.push(x + y);//取出堆顶的两个元素，求和后雅俗优先队列
		ans += (x + y);//累计求和结果 
	} 
	printf("%lld",ans);//ans为消耗体力的最小值 
	return 0;
}
```

> 哈夫曼编码；确定出现的字符串 -> 每个字符出现的频率 -> 建立哈夫曼树 -> 形成哈夫曼编码
>
> 代码比较难实现，有时间再研吧

### 十七、图

#### 1、图的存储方式：

1、邻接矩阵：设图G(V,E)的顶点标号为0，1，~ N- 1，那么令二维数组`G[N][N]`表示图的顶点标号，`G[i][j]`表示顶点i到顶点j右边，置于边权是多少，可以自行设定，局限性就在于数组不能开的太大，一般顶点数不能太大（上限为1000）

2、邻接表:把同一个顶点所有出现的边放到一个列表中，那么N个顶点就会有n个列表，这N个列表被称为图G的邻接表，记为Adj[N],其中Adj[0] ~Adj[N-1],就分别是一个列表

```c
//如果邻接表只放每条表终点编号，而且不放边权，可以直接这样定义
vector<int> Adj[N];
Adj[0].push_back(i);~Adj[N-1].push_back(i);
如果再1和3这两个边设为无向边，这样做;
Adj[1].push_back(3),Adj[3].push_back(1);这样就是无向边了
//如果同时存放边的终点编号和边权
struct node{
    int v;//边的终点编号
    int w;//边权
};
vector<node> Adj[N];
如果添加1号到3号的有向边，边权为4，这样就用到临时变量
Node temp;
temp.v = 3;
temp.w = 4;
Adj[1].push_back(temp);
```

#### 2、图的遍历

> 1、连通分量：`无向图中`如果两个顶点可以互相到达（直接或间接），那么称这两个顶点连通；如果图G(V，E)的任意两个顶点都连通，那么称改图是连通图；  `非连通图`；图G(V,E)的任意两个顶点是不连通的，那么称改图为非连通图，并且非连通子图产生的子集（它的子集是连通图，子集有很多），即为连通分量
>
> 2、强连通分量：`有向图中`；如果两个顶点可以各自通过一条有向路径到达另一个顶点（互相到达），就称这两个顶点强连通， `非强连通图`；图G(V,E)的任意两个顶点是不强连通的，那么称改图为非强连通图，并且非强连通子图产生的子集（它的子集是强连通子集，子集有很多，一个点也算是强连通分量），即为连通分量
>
> 连通分量和强连通分量统称为连通块
>
> ==可以解决的问题==；判断图中有几个连通图

1、dfs遍历模板

```c++
//邻接矩阵版 （大致模板）
const int maxv = 1000;//最大顶点数 
const int INF = 1000000000;//设INF为一个很大的数 
int n, G[maxv][maxv];//n为顶点数
bool vis[maxv] = {false};//如果该顶点被访问，则vis[i] = true

void dfs(int u, int depth)//u为当前访问的顶点标号，depth为深度
{
	vis[u] = true;//设置u被访问
	//如果要对u操作，可以再这里进行
	//下面对所有从u出发能到达的分支顶点进行枚举
	for(int v = 0; v < n; v++) //对每个顶点v
	{
		if(vis[v] == false && G[u][v] != INF) //如果该顶点没被访问，且u和v之间有边 
		{
			dfs(v,depth + 1);//访问v,并深度加1 
		}
	} 
} 
void dfsTrave() //遍历图G
{
	for(int u = 0; u < n; u++) //对每个顶点u 
	{
		if(vis[u] == false)//如果u未被访问 
		{
			dfs(u,1);//访问u和u所在的连通块，1表示初始为第一层 
		}
	}
}


//邻接表版 大致模板
const int maxv = 1000;//最大顶点数 
const int INF = 1000000000;//设INF为一个很大的数 
int n;//n为顶点数
bool vis[maxv] = {false};//如果该顶点被访问，则vis[i] = true
vector< int > Adj[maxv];//图G的邻接表 
void dfs(int u, int depth)//u为当前访问的顶点标号，depth为深度
{
	vis[u] = true;//设置u被访问
	//如果要对u操作，可以再这里进行
	//下面对所有从u出发能到达的分支顶点进行枚举
	for(int i = 0; i < Adj[u].size(); i++) //对每个顶点v
	{
		int v = Adj[u][i];
		if(vis[v] == false ) //如果该顶点没被访问，且u和v之间有边 
		{
			dfs(v,depth + 1);//访问v,并深度加1 
		}
	} 
} 
void dfsTrave() //遍历图G
{
	for(int u = 0; u < n; u++) //对每个顶点u 
	{
		if(vis[u] == false)//如果u未被访问 
		{
			dfs(u,1);//访问u和u所在的连通块，1表示初始为第一层 
		}
	}
} 
```

2、bfs遍历模板

```c++
//邻接矩阵大致模板
const int maxv = 1000;
int n, G[maxv][maxv];
bool inq[maxv] = {false};//顶点i层入过队列，则inq[i] = true

void bfs(int u) // 遍历u所在的连通块
{
	queue<int> q;
	q.push(u);
	inq[u] = true;
	while(!q.empty())
	{
		int u = q.front();
		q.pop();
		for(int v = 0; v < n; v++)
		{
			if(inq[v] == false && G[u][v] != INF)
			{
				q.push(v);
				inq[v] = true;
			}
		}
	}
} 
void bfsTrave()
{
	for(int u = 0; u < n; u ++) //枚举所有顶点 
	{
		if(inq[u] == false) //u未曾入过队列 
		{
			bfs(u);//遍历u所在的连通块 
		}
	}
}

//邻接表版大致模板
const int maxv = 1000;
int n;
vector<int> Adj[maxv]; 
bool inq[maxv] = {false};//顶点i层入过队列，则inq[i] = true

void bfs(int u) // 遍历u所在的连通块
{
	queue<int> q;
	q.push(u);
	inq[u] = true;
	while(!q.empty())
	{
		int u = q.front();
		q.pop();
		for(int i = 0; i < Adj[u].size(); i++)
		{
			int v = Adj[u][i];
			if(inq[v] == false )
			{
				q.push(v);
				inq[v] = true;
			}
		}
	}
} 
void bfsTrave()
{
	for(int u = 0; u < n; u ++) //枚举所有顶点 
	{
		if(inq[u] == false) //u未曾入过队列 
		{
			bfs(u);//遍历u所在的连通块 
		}
	}
}
//邻接表出现了层号
struct node{
	int v;//顶点编号
	int layer;//顶点层号 
};

const int maxv = 1000;
int n;
vector<node> Adj[maxv]; 
bool inq[maxv] = {false};//顶点i层入过队列，则inq[i] = true

void bfs(int u) // 起始顶点编号 
{
	queue<node> q;
	node start;//起始顶点
	start.v = u;
	start.layer = 0; 
	q.push(start);
	inq[start.v] = true;
	while(!q.empty())
	{
		node temp = q.front();
		q.pop();
		int u = temp.v;//队首顶点编号 
		for(int i = 0; i < Adj[u].size(); i++)
		{
			node next = Adj[u][i];//u出发能达到顶点next
			next.layer = temp.layer + 1; 
			if(inq[next.v] == false )
			{
				q.push(next);
				inq[next.v] = true;
			}
		}
	}
} 
void bfsTrave()
{
	for(int u = 0; u < n; u ++) //枚举所有顶点 
	{
		if(inq[u] == false) //u未曾入过队列 
		{
			bfs(u);//遍历u所在的连通块 
		}
	}
}
```

实例

> 每个输入文件包含若干行，每行两个整数i,j，表示节点i和j之间存在一条边。,输出每个图的联通分支数

```c
#include <algorithm>
#include <cstdio>
#include <queue>
#include <vector>
#include <iostream>
using namespace std;

const int maxv = 1000010;
vector<int>adj[maxv];
bool inq[maxv] = { false };
bool Hash[maxv] = { false }; //增加一个hash，来判断改图有哪些点
void bfs(int u)
{
	queue<int>q;
	q.push(u);
	inq[u] = true;
	while(!q.empty())
	{
		int u = q.front();
		q.pop();
		for (int i = 0; i < adj[u].size(); i++)
		{
			int v = adj[u][i];
			if (inq[v] == false)
			{
				q.push(v);
				inq[v] = true;
			}
		}
	}
}
void bfsTrave(int n)
{
	int count = 0;
	for (int u = 0; u <= n; u++)
	{
		if (Hash[u] == true && inq[u] == false)
		{
			bfs(u);
			count++;
		}
	}
	cout << count;
}
int main()
{
	int n = 0; //记录最大顶点，好往下依次遍历,寻找定点数，因为好多顶点不确定有没有，所以用hash来记录由那些顶点，如果给出确实的n,就不用hash表了,如下一题
	int a, b;
	while (scanf("%d %d", &a, &b) != EOF)
	{
		if (n < b) n = b;
		if (n < a) n = a;
		adj[a].push_back(b);
		adj[b].push_back(a); //无向图
		Hash[a] = Hash[b] = true;
	}
	bfsTrave(n);
	return 0;
}
```

> 给定一个无向图和其中的所有边，判断这个图是否所有顶点都是连通的，输入：每组数据的第一行是两个整数 n 和 m（0<=n<=1000）。n 表示图的顶点数目，m 表示图中边的数目。如果 n 为 0 表示输入结束。随后有 m 行数据，每行有两个值 x 和 y（0<x, y <=n），表示顶点 x 和 y 相连，顶点的编号从 1 开始计算。输入不保证这些边是否重复
>
> 输出：对于每组输入数据，如果所有顶点都是连通的，输出"YES"，否则输出"NO"。

```c
#include <iostream>
#include <cstdio>
#include <string>
#include <vector>
#include <queue>
#include <cstring>
#include <algorithm>
using namespace std;

const int maxv = 1010;
int n, m;
vector<int>adj[maxv];
bool inq[maxv] = { false };


void bfs(int u)
{
	queue<int>q;
	q.push(u);
	inq[u] = true;
	while (!q.empty())
	{
		int u = q.front();
		q.pop();
		for (int i = 0; i < adj[u].size(); i++)
		{
			int v = adj[u][i];
			if (inq[v] == false )
			{
				q.push(v);
				inq[v] = true;
			}
		}
	}
}

int bfsTrave()
{
	int count = 0;
	for (int u = 1; u <= n; u++)
	{
		if ( inq[u] == false)
		{
			bfs(u);
			count++;
		}
	}

	return count;
}
int main()
{
	int x, y;
	while (scanf("%d %d", &n, &m) && n != 0) {
		for (int i = 1; i < maxv; i++)
		{
			adj[i].clear();
			inq[i] = false;
		}
		int z = 0;
		for (int i = 0; i < m; i++)
		{
			cin >> x >> y;
			adj[x].push_back(y);
			adj[y].push_back(x);
		}
		int ans = bfsTrave();
		if (ans == 1 || (n == 1&&m == 0)) cout << "YES" << endl;
		else cout << "NO" << endl;
	}
	return 0;
}

```



#### 3、最短路径

##### 1、Dijkstra(迪杰斯特拉算法)

> ==解决单源最短路径问题==它的算法策略是：
>
> 1、每次从集合V-S未被攻占的城市中选则起点s的最短距离最小的一个顶点（即为u）,访问并加入集合S(即已被攻占)
>
> 2、令顶点u为中介点，优化起点s与所有从u能到达的顶点V之间的最短路径
>
> ==这个算法的瑕疵在于不能解决边权是负值的情况==

```c
//邻接矩阵的大致模板，起点s到各个点的最小路径
//邻接矩阵 
const int maxv = 1000;
const int inf = 0x3fffffff; 
int n, G[maxv][maxv];//顶点数，maxv为最大顶点数 
int d[maxv]; //起点到各店的最短路径长度
bool vis[maxv] = {false};
void Dijkstra(int s) //s为起点
{
	fill(d,d + maxv, inf);
	d[s] = 0; //起点s到达自身的距离为0
	for(int i = 0; i < n; i++) //循环n次 
	{
		int u = -1, minn = inf; //u是d[u]最小，minn存放该最小的d[u]
		for(int j = 0; j < n; j++)
		{
			if(vis[j] == false && d[j] < minn)
			{
				u = j;
				minn = d[j];
			}
		} 
		//找不到小于inf的d[u],说明剩下的顶点和起点s不连通
		if(u == -1) return ;
		vis[u] = true;
		for(int v = 0; v < n; v++)
		{
			//v未被访问&&能到达v&&以u为中介点可以使d[v]更优
			if(vis[v] == false && G[u][v] != inf && d[v] > d[u] + G[u][v])
			{
				d[v] = d[u] + G[u][n];//优化d[v] 
			} 
		} 
	} 
} 
```

````c++
//邻接表版，大致模板 
struct node{
	int v;//v为目标顶点 
	int dis; //dis为边权 
}; 
const int maxv = 1000;
const int inf = 0x3fffffff; 
vector<node> Adj[maxv];//图G,Adj[u]存放等点u出发可以到达的所有顶点 
int n;//顶点数
int d[maxv]; //起点到各店的最短路径长度
bool vis[maxv] = {false};
void Dijkstra(int s) //s为起点
{
	fill(d,d + maxv, inf);
	d[s] = 0; //起点s到达自身的距离为0
	for(int i = 0; i < n; i++) //循环n次 
	{
		int u = -1, minn = inf; //u是d[u]最小，minn存放该最小的d[u]
		for(int j = 0; j < n; j++)
		{
			if(vis[j] == false && d[j] < minn)
			{
				u = j;
				minn = d[j];
			}
		} 
		//找不到小于inf的d[u],说明剩下的顶点和起点s不连通
		if(u == -1) return ;
		vis[u] = true;
		for(int j = 0; j < n; j++)
		{
			int v = Adj[u][j].v;
			if(vis[v] == false && d[v] > d[u] + Adj[u][j].dis)
			{
				d[v] = d[u] + Adj[u][j].dis;//优化d[v] 
			} 
		} 
	} 
} 
````

实例

> 有n个城市m条道路（n<1000, m<10000)，每条道路有个长度，请找到从起点s到终点t的最短距离和经过的城市名。输入：输入包含多组测试数据。
>
> 每组第一行输入四个数，分别为n，m，s，t。
>
> 接下来m行，每行三个数，分别为两个城市名和距离。
>
> 输出：每组输出占两行。
>
> 第一行输出起点到终点的最短距离。
>
> 第二行输出最短路径上经过的城市名，如果有多条最短路径，输出字典序最小的那条。若不存在从起点到终点的路径，则输出“can't arrive”。

```c
#include <algorithm>
#include <cstdio>
#include <queue>
#include <vector>
#include <iostream>
using namespace std;
struct node {
    int v;//顶点
    int dis;//边权
    node(int x, int y) :v(x), dis(y) { };
};
const int inf = 0x3fffffff;
const int maxv = 1010;
//int G[maxv][maxv];
bool visit[maxv] = { false };
vector<node>adj[maxv];
int n, m, s, t;
int pre[maxv];
int d[maxv];

void Dijkstra(int s)
{
    fill(d, d + maxv, inf);
    fill(visit, visit + maxv, false); //这个很有必要。
    for (int i = 1; i <= n; i++)
    {
        pre[i] = i;
    }
    d[s] = 0;
    for (int i = 1; i <= n; i++)
    {
        int u = -1, minn = inf;
        for (int j = 1; j <= n; j++)
        {
            if (visit[j] == false && d[j] < minn)
            {
                minn = d[j];
                u = j;
            }
        }
        if (u == -1)  return;
        visit[u] = true;
        for (int j = 0; j < adj[u].size(); j++)
        {
            int v = adj[u][j].v;
            int dis = adj[u][j].dis;
            if (visit[v] == false)
            {
                if (d[v] > d[u] + dis)
                {
                    d[v] = d[u] + dis;
                    pre[v] = u;
                }
                else if (d[v] == d[u] + dis && u < pre[v]) {
                    pre[v] = u;
                }
            }
        }
    }

}

void dfs(int s , int t)
{
    if (s == t)
    {
        printf("%d ", t);
        return;
    }
    dfs(s, pre[t]);
    printf("%d ", t);
}
int main()
{
    while (scanf("%d %d %d %d", &n, &m, &s, &t) != EOF)
    {
        int x, y, z;
        for (int i = 1; i <= n; i++)
        {
            adj[i].clear();
        }
        for (int i = 0; i < m; i++)
        {
            cin >> x >> y >> z;
            adj[x].push_back(node(y,z));
            adj[y].push_back(node(x, z));
        }
        Dijkstra(s);
        if (d[t] == inf) {
            printf("can't arrive\n");
        }
        else {
            printf("%d\n", d[t]);
            dfs(s,t);
            cout << endl;
        }
        
    }
    return 0;
}
```

实例；从顶点v0到达每个顶点的最短路径（单源路径）

```c
#include <algorithm>
#include <cstdio>
#include <queue>
#include <vector>
#include <iostream>
using namespace std;

const int MAXV = 60;
const int inf = 0x3fffffff;
int n, s, G[MAXV][MAXV];
bool visit[MAXV] = { false };
int d[MAXV];
void Dijkstra(int s)
{
	fill(d, d + MAXV, inf);
	d[s] = 0;
	for (int i = 0; i < n; i++)
	{
		int u = -1, minn = inf;
		for (int j = 0; j < n; j++)
		{
			if (visit[j] == false && d[j] < minn)
			{
				minn = d[j];
				u = j;
			}
		}

		if (u == -1) {
			return;
		}
		visit[u] = true;
		for (int v = 0; v <  n; v++)
		{
			if (visit[v] == false && G[u][v] != inf && d[v] > d[u] + G[u][v])
			{
				d[v] = d[u] + G[u][v];
			}
		}
	}
}

int main()
{
	cin >> n >> s;
	int temp = 0;
	fill(G[0], G[0] + MAXV * MAXV, inf);
	for (int i = 0; i <  n; i++)
	{
		for (int j = 0; j < n; j++)
		{
			cin >> temp;
			if (temp > 0) G[i][j] = temp;
		}
	}
	Dijkstra(s);
	for (int i = 0; i < n; i++)
	{
		if (d[i])
		{
			if (d[i] < inf) printf("%d", d[i]);
			else printf("-1");
			if (i < n - 1) cout << " ";
		}

	}
	return 0;
}
```

> ==上面的实例时有向图，如果要是无向图的话，对于邻接矩阵，G[u] [v] = G[v] [u] = w; 对于邻接表，Adj[u].push_back(v),Adj[v].push_back(u)即可完成无向图==

具体求出从原点到某个点的最短路径时什么（用递归）

```c
//邻接矩阵的大致模板，起点s到某个点的最短路径
const int maxv = 1000;
const int inf = 0x3fffffff; 
int n, G[maxv][maxv];//顶点数，maxv为最大顶点数 
int d[maxv]; //起点到各店的最短路径长度
bool vis[maxv] = {false};
int pre[maxv] ;//pre[v]表示从起到到顶点v的最短路径v上的一个前一个顶点 

void Dijkstra(int s) //s为起点
{
	fill(d,d + maxv, inf);
	d[s] = 0; //起点s到达自身的距离为0
	for(int i = 0; i < n; i++) //循环n次 
	{
		int u = -1, minn = inf; //u是d[u]最小，minn存放该最小的d[u]
		for(int j = 0; j < n; j++)
		{
			if(vis[j] == false && d[j] < minn)
			{
				u = j;
				minn = d[j];
			}
		} 
		//找不到小于inf的d[u],说明剩下的顶点和起点s不连通
		if(u == -1) return ;
		vis[u] = true;
		for(int v = 0; v < n; v++)
		{
			//v未被访问&&能到达v&&以u为中介点可以使d[v]更优
			if(vis[v] == false && G[u][v] != inf && d[v] > d[u] + G[u][v])
			{
				d[v] = d[u] + G[u][n];//优化d[v]
				pre[v] = u; 
			} 
		} 
	} 
} 
void dfs(int s, int v)//s为起点编号，v为当前访问的顶点编号（从终点开始递归） 
{
	if(s == v) //如果当前已经到达起点s,则输出起点并返回 
	{
		printf("%d",s);
		return;
	}
	dfs(s,pre[v]);//递归访问v的前驱顶点pre[v] 
	printf("%d ",v);//从最深处return回来后，输出每一层的顶点号 
}
```

问题一；给每条边增加一个边权（边权代表这条路的花费），在求最短路径时，要求花费最少

```c
//cost[u][v]表示u->v的花费，有题目给出
//c[maxv],表示每条路的花费最小值，c[s] = 0,其他都等于inf，
for(int v = 0; v < n; v++)
		{
			//v未被访问&&能到达v&&以u为中介点可以使d[v]更优
			if(vis[v] == false && G[u][v] != inf )
			{
				if(d[v] > d[u] + G[u][n]) {
					d[v] = d[u] + G[u][n];//优化d[v]
					c[v] = c[u] + cost[u][v];
				} else if(d[u] + G[u][v] == d[v] && c[u] + cost[u][v] < c[v]){
					c[v] = c[u] + cost[u][v];//最短距离相同时看能否时c[v]更优 
				}
			} 
		}
```

问题二；给每个点增加一个点权（边权代表该城市收集到多少物资），在求最短路径时，要求（s->v）的物资最大

```c
//weight[u]表示城市u中的物资数目，有题目给出
//w[maxv],表示每个城市的物资最大值，w[s] = weight[s],其他都等于0，
for(int v = 0; v < n; v++)
		{
			//v未被访问&&能到达v&&以u为中介点可以使d[v]更优
			if(vis[v] == false && G[u][v] != inf )
			{
				if(d[v] > d[u] + G[u][n]) {
					d[v] = d[u] + G[u][n];//优化d[v]
					w[v] = w[u] + weight[v];
				} else if(d[u] + G[u][v] == d[v] && w[u] + weight[v] > w[v]){
					w[v] = w[u] + weight[v];//最短距离相同时看能否时w[v]更优 
				}
			} 
		} 
```

问题三，求最短路径条数

> 只需要增加一个数组num[],初始化num[s] = 1,其余都为0，没有相同的路径，只需要num[v] = num[u];（继承前驱），但是有相同的最短路径，num[v] += num[u];（最短距离相同时累加num）

```c
	for(int v = 0; v < n; v++)
		{
			//v未被访问&&能到达v&&以u为中介点可以使d[v]更优
			if(vis[v] == false && G[u][v] != inf )
			{
				if(d[v] > d[u] + G[u][n]) {
					d[v] = d[u] + G[u][n];//优化d[v]
				    num[v] = num[u];
				} else if(d[u] + G[u][v] == d[v] ){
				    num[v] += num[u];//最短距离相同时累加num 
				}
			} 
		} 
```

实例

> 题目；给出N个城市，M条无向边，每个城市有一定数目救援小组，（边权已知）现给出起点和终点，求从起点到终点的最短路径条数及最短路径上的救援小组数目之和

```c
#include <iostream>
#include <algorithm>
#include <cstring>
#include <vector>
#include <cstdio>
using namespace std;

const int maxv = 510;
const int inf = 0x3fffffff; 

int n ,m ,st, ed, G[maxv][maxv], weight[maxv];

int d[maxv]; //起点到各店的最短路径长度
int w[maxv], num[maxv];
bool vis[maxv] = {false};

void Dijkstra(int s)
{
	fill(d,d + maxv,inf);
	memset(num,0,sizeof(num));
	memset(w,0,sizeof(w));
	d[s] = 0;
	w[s] = weight[s];
	num[s] = 1;
	for(int i = 0; i < n; i++)
	{
		int u = -1, minn = inf;
		for(int j = 0; j < n; j++)
		{
			if(vis[j] == false && d[j] < minn)
			{
				u = j;
				minn = d[j];
			}
		}
		
		if(u == -1) return ;
		vis[u] = true;
		
		for(int v = 0; v < n; v++)
		{
			if(vis[v] == false && G[u][v] != inf)
			{
				if(d[v] > d[u] + G[u][v])
				{
					d[v] = d[u] + G[u][v];
					w[v] = w[u] + weight[v];
					num[v] = num[u]; 
				} else if(d[v] == d[u] + G[u][v]) {
					if(w[u] + weight[v] > w[v])
					{
						w[v] = w[u] + weight[v];
					}
					num[v] += num[u];
				}
			}
		}
	}
}

int main()
{
    scanf("%d %d %d",&n ,&m ,&st, &ed);//顶点数量，边数，起点，终点
	for(int i = 0; i < n; i++)
	{
		scanf("%d",&weight[i]);//读入点权 
	} 
	int u, v;
	fill(G[0],G[0] + maxv * maxv,inf);
	for(int i = 0; i < m; i++)
	{
		scanf("%d %d",&u,&v);
		scanf("%d",&G[u][v]);
		G[v][u] = G[u][v];//关于这里的赋值千万不能写反 
	} 
	Dijkstra(st);
	printf("%d %d\n",num[ed],w[ed]);
	return 0;
} 
```

实例

> 给你n个点，m条无向边，每条边都有长度d和花费p，给你起点s终点t，要求输出起点到终点的最短距离及其花费，如果最短距离有多条路线，则输出花费最少的。输入：输入n,m，点的编号是1~n,然后是m行，每行4个数 a,b,d,p，表示a和b之间有一条边，且其长度为d，花费为p。最后一行是两个数 s,t;起点s，终点t。n和m为0时输入结束。
> (1<n<=1000, 0<m<100000, s != t)
>
> 输出：输出 一行有两个数， 最短距离及其花费。

```c
#include <iostream>
#include <vector>
#include <cstdio>
#include <algorithm>
#include <cstring>
using namespace std;

const int inf = 0x3fffffff;
const int maxv = 1010;
struct node {
	int v;//顶点
	int d;//长度
	int p;//花费
	node(int x, int y, int z) :v(x), d(y), p(z) { };
};
vector<node>adj[maxv];
int cost[maxv];
int d[maxv];
bool visit[maxv];
int n, m, s, t;
void Dijkstra(int s)
{
	fill(d, d + maxv, inf);
	fill(visit, visit + maxv, false);
	fill(cost, cost + maxv, inf);
	for (int i = 0; i < adj[s].size(); i++)
	{
		int v = adj[s][i].v;
		cost[v] = adj[s][i].p;
		d[v] = adj[s][i].d;
	}
	d[s] = 0;
	for (int i = 0; i < n; i++)
	{
		int u = -1, minn = inf;
		for (int j = 0; j < n; j++)
		{
			if (visit[j] == false && d[j] < minn)
			{
				minn = d[j];
				u = j;
			}
		}
		if (u == -1) return;
		visit[u] = true;
		for (int j = 0; j < adj[u].size(); j++)
		{
			int v = adj[u][j].v;
			int p = adj[u][j].p;
			int dis = adj[u][j].d;
			if (visit[v] == false)
			{
				if (d[v] > d[u] + dis)
				{
					d[v] = d[u] + dis;
					cost[v] = cost[u] + p;
				}
				else if (d[v] == d[u] + dis && cost[v] > cost[u] + p)
				{
					cost[v] = cost[u] + p;
				}
			}
		}
	}

}

int main()
{
	while (scanf_s("%d %d", &n, &m) != EOF && n != 0 && m != 0)
	{
		for (int i = 0; i < n; i++)
		{
			adj[i].clear();
		}
		int a, b, temp, p;
		for (int i = 0; i < m; i++)
		{
			cin >> a >> b >> temp >> p;
			adj[a].push_back(node(b, temp, p));
			adj[b].push_back(node(a, temp, p));
		}
		cin >> s >> t;
		Dijkstra(s);
		printf("%d %d\n",d[t],cost[t]);
	}
	return 0;
}
```



多个最短路径中的最优路径

> 设置vector<int> pre[maxv],当一个点有多个相同最短路径时，全部存储，关于遍历,找到叶子节点，把它的（点/边权值相加），跟最右端权值比较，最后确定一个最优的路径

```c
//最优路径和dfs
int optvalue;//多个最短路径的最优值 
vector<int> path, temppath;//最优路径，临时路径
vector<int>pre[maxv];

void Dijkstra(int s)
{
	fill(d,d + maxv,inf); 
	d[s] = 0;
	for(int i = 0; i < n; i++)
	{
		int u = -1, minn = inf;
		for(int j = 0; j < n; j++)
		{
			if(vis[j] == false && d[j] < minn)
			{
				u = j;
				minn = d[j];
			}
		}
		
		if(u == -1) return ;
		vis[u] = true;
		
		for(int v = 0; v < n; v++)
		{
			if(vis[v] == false && G[u][v] != inf)
			{
				if(d[v] > d[u] + G[u][v])
				{
					d[v] = d[u] + G[u][v];
					pre[v].clear();
					pre[v].push_back(u);
				} else if(d[v] == d[u] + G[u][v]) {
					pre[v].push_back(u);
				}
			}
		}
	}
}

void dfs(int v)//当前节点
{
	//递归边界
	if(v == st) //如果到达路径的叶子节点st,(也就是路径的起点) 
	{
		temppath.push_back(v);//将起点st加入临时路径temppath最后面 
		int value = 0; //存放临时路径temppath的最短路径的值
		//计算边权之和
		for(int i = temppath.size() - 1; i>0; i--)
		{
			//当前节点id,下一结点idnext
			int id = temppath[i],idnext = temppath[i - 1];
			value += V[id][idnext];//V为边权 
		}
		/*
		//计算点权之和 
		for(int i = temppath.size() - 1; i > 0; i--)
		{
		    int id = temppath[i];
		    value += W[id];//W[id]为点权 
	    }
		*/ 
		if(value > optvalue)
		{
			optvalue = value;
			path = temppath;
		}
		temppath.pop_back();//将刚加入的结点删除 
		return;
	}
	//递归式子
	temppath.push_back(v);
	for(int i = 0; i < pre[v].size(); i++)
	{
		dfs(pre[v][i]);
	} 
	temppath.pop_back();//遍历完所有前驱节点，将当前节点v清除 
} 
```

实例

> 题目描述：有N个城市，M条道路（无向边），并给出M条道路的属性距离和花费属性，献给定起点s和终点D,求从起点到终点的最短路径，和最短距离的花费，多个最短路径，选则花费最少的那条

```c
#include <iostream>
#include <algorithm>
#include <cstring>
#include <vector>
#include <cstdio>
using namespace std;

const int maxv = 510;
const int inf = 0x3fffffff; 

int n, m , st, ed, G[maxv][maxv],cost[maxv][maxv];
int d[maxv], mincost = inf;//最优的花费 
bool vis[maxv] = {false}; 
vector<int> path, temppath;//最优路径，临时路径
vector<int>pre[maxv];

void Dijkstra(int s)
{
	fill(d,d + maxv,inf); 
	d[s] = 0;
	for(int i = 0; i < n; i++)
	{
		int u = -1, minn = inf;
		for(int j = 0; j < n; j++)
		{
			if(vis[j] == false && d[j] < minn)
			{
				u = j;
				minn = d[j];
			}
		}
		
		if(u == -1) return ;
		vis[u] = true;
		
		for(int v = 0; v < n; v++)
		{
			if(vis[v] == false && G[u][v] != inf)
			{
				if(d[v] > d[u] + G[u][v])
				{
					d[v] = d[u] + G[u][v];
					pre[v].clear();
					pre[v].push_back(u);
				} else if(d[v] == d[u] + G[u][v]) {
					pre[v].push_back(u);
				}
			}
		}
	}
}

void dfs(int v)//当前节点
{
	//递归边界
	if(v == st) //如果到达路径的叶子节点st,(也就是路径的起点) 
	{
		temppath.push_back(v);//将起点st加入临时路径temppath最后面 
		int tempcost = 0; //存放临时路径temppath的最短路径的值
		//计算边权之和
		for(int i = temppath.size() - 1; i > 0; i--)
		{
			//当前节点id,下一结点idnext
			int id = temppath[i],idnext = temppath[i - 1];
			tempcost += cost[id][idnext];//V为边权 
		}
		if(tempcost < mincost)
		{
			mincost = tempcost;
			path = temppath;
		}
		temppath.pop_back();//将刚加入的结点删除 
		return;
	}
	//递归式子
	temppath.push_back(v);
	for(int i = 0; i < pre[v].size(); i++)
	{
		dfs(pre[v][i]);
	} 
	temppath.pop_back();//遍历完所有前驱节点，将当前节点v清除 
} 
int main()
{
    scanf("%d %d %d",&n ,&m ,&st, &ed);//顶点数量，边数，起点，终点
	int u, v;
	fill(G[0],G[0] + maxv * maxv,inf);
	fill(cost[0],cost[0] + maxv*maxv,inf);
	for(int i = 0; i < m; i++)
	{
		scanf("%d %d",&u,&v);
		scanf("%d %d",&G[u][v], &cost[u][v]);
		G[v][u] = G[u][v];//关于这里的赋值千万不能写反 
	    cost[v][u] = cost[u][v];
	} 
	Dijkstra(st);
	dfs(ed);//获取最优路径 
	for(int i = path.size() - 1; i >= 0; i--)
	{
		printf("%d ",path[i]);
	}
	printf("%d %d\n",d[ed],mincost);
	return 0;
} 
```

##### 2、Bellman-Ford（B-F）算法和SPFA算法

> 

```c
const int maxv = 510;
const int inf = 0x3fffffff;
int n;

struct node{
	int v;//邻接表的目标定点 
	int dis;//邻接边的边权 
};

vector< node > Adj[maxv];//图G的邻接表 
int d[maxv]; //最短路径的长度 

bool Bellman(int s) //s为起点
{
	fill(d, d + maxv, inf);
	d[s] = 0; //自身到自身的距离为0
	//以下是求解数组d的部分
	for(int i = 0; i < n - 1; i++)//执行n- 1轮操作，n为顶点数 
	{
		for(int u = 0; u < n; u++)//每轮操作都遍历所有边 
		{
			for(int j = 0; j < Adj[u].size(); j++)
			{
				int v = Adj[u][j].v;//顶点 
				int dis = Adj[u][j].dis;//边权 
				if(d[v] > d[u] + dis) //以u为中介点可以使d[v]更优 
				{
					d[v] = d[u] + dis;//松弛操作 
				}
			}
		}
	} 
	//以下为判断负环的代码
	for(int u = 0; u < n; u++)
	{
		for(int j = 0; j < Adj[u].size(); j++)
		{
			int v = Adj[u][j].v;
			int dis = Adj[u][j].dis
			if(d[v] > d[u] + dis){//如果仍可以被松弛
			    return false; //说明图中有从原点可达的负环 
			} 
		}
	} 
	return true;
} 
```

> 判断有多少个最短路径，还是用SPFA来判断比较好

```c
const int maxv = 510;
const int inf = 0x3fffffff;

int n;
int num[maxv]; //num数组记录顶点的入队次数 
bool inq[maxv]; //顶点是否在队列中 
vector< node > Adj[maxv];//图G的邻接表 
int d[maxv]; //最短路径的长度 
bool SPFA(int s)
{
	memset(inq, false, sizeof(inq));
	memset(num,0,sizeof(num));
	fill(d, d + maxv, inf);
	//原点入队部分
	queue<int>q;
	q.push(s);
	inq[s] = true;
	num[s]++;
	d[s] = 0;
	//主体部分
	while(!q.empty()) 
	{
		int u = q.front();
		q.pop();
		inq[u] = false; //设u不在队列中 
		//编历u的所有邻接边v
		for(int j = 0; j < Adj[u].size();j++)
		{
			int v = Adj[u][j].v;
			int dis = Adj[u][j].dis
			//松弛操作
			if(d[u] + dis < d[v])
			{
				d[v] = d[u] + dis;
				if(!inq[v]) { //如果v不在队列中 
					q.push(v);//v入队
					inq[v] = true;
					num[v] ++;
					if(num[v] >= n) return false;//有可达负环 
				}
			} 
		} 
	}
	return true;//无可达负环 
} 
```





##### 3、Floyed算法

> Floyed算法；弗洛伊德算法，==解决全源最短路问题==，即任意给定两个点，求出这两个点的最短路径，而不仅仅是起点对于其它点

```c
#include <iostream>
#include <algorithm>
#include <cstring>
#include <vector>
#include <cstdio>
#include <string>
#include <cmath>
#include <queue>
using namespace std;
const int maxv = 510;
const int inf = 0x3fffffff;
int n, m;//顶点数，m为边数
int dis[maxv][maxv]; //dis[i][j]表示顶点i,到顶点j的最短距离 

void floyed()
{
	for(int k = 0; k < n; k++)
	{
		for(int i = 0; i < n; i++)
		{
			for(int j = 0; j < n; j++)
			{
				if(dis[i][k] != inf && dis[k][j] != inf && dis[i][j] > dis[i][k] + dis[k][j])
				{
					dis[i][j] = dis[i][k] + dis[k][j];
				}
			}
		}
	}
} 
int main()
{
    int u, v, w;
    fill(dis[0],dis[0] + maxv*maxv,inf);
    scanf("%d %d",&n, & m);//顶点数，边数
	for(int i = 0; i < n; i++)
	{
		dis[i][i] = 0; //顶点i到顶点j的距离为0 
	}
	for(int i = 0; i < m; i++)
	{
		scanf("%d %d %d",&u, & v,&w);
		dis[u][v] = w;
	} 
	floyed();
	for(int i = 0; i < n; i++)
	{
		for(int j = 0; j < n; j++)
		{
			printf("%d ",dis[i][j]);
		}
		cout << endl;
	}
	return 0;
} 
```

实例

```c
#include <algorithm>
#include <cstdio>
#include <queue>
#include <vector>
#include <iostream>
using namespace std;
int n, s;
const int MAXV = 60;
const int inf = 0x3fffffff;
bool visit[MAXV] = { false };
int d[MAXV][MAXV];
void Floyed()
{
	
	for (int k = 0; k < n; k++)
	{
		for (int i = 0; i < n; i++)
		{
			for (int j = 0; j < n; j++)
			{
				if (d[i][k] != inf && d[k][j] != inf && d[i][j] > d[i][k] + d[k][j])
				{
					d[i][j] = d[i][k] + d[k][j];
				}
			}
		}
	}
}

int main()
{
	fill(d[0], d[0] + MAXV * MAXV, inf);
	cin >> n;
	int temp = 0;
	for (int i = 0; i <  n; i++)
	{
		for (int j = 0; j < n; j++)
		{
			scanf("%d", &d[i][j]);
			if (i != j && d[i][j] == 0) d[i][j] = inf;
		}
	}
	Floyed();
	for (int i = 0; i < n; i++)
	{
		for (int j = 0; j < n; j++)
		{
			if (d[i][j] == inf) printf("-1");
			else printf("%d", d[i][j]);
			if (j < n - 1) cout << " ";
		}
		cout << endl;

	}
	return 0;
}

```



#### 4、最小生成树

> 最小生成树性质：==首先它是一个无向图==，性质一；最小生成树是树，边数等于顶点数减1，树内一定不会有环， 性质二；最小生成树不唯一，但是边权值和一定为1； 性质三；通过给定一个结点对最小生成树输出其结果就唯一了

##### 1.prim算法

普利姆算法

> 算法核心==设置一个集合s（巨型防护罩）,和为访问结点，通过巨型防护罩去找围在罩里面的结点，看看罩到那个结点的路径最短，并让其加入到这个罩中，直到顶点全部到罩里面

```c
//邻接矩阵
const int maxv = 510;
const int inf = 0x3fffffff;


int n, G[maxv][maxv];
int d[maxv]; //顶点与集合s的最短距离
bool vis[maxv] = {false};
int prim() //默认0号为初始点，函数返回最小生成树的边权之和
{
	int minn;
	fill(d,d + maxv,inf);
	d[0] = 0;
	int ans = 0; //存放最小生成树的边权之和
	for(int i = 0; i < n; i++)
	{
		int u = -1,minn = inf;
		for(int j = 0; j < n; j++)
		{
			if(vis[j] == false && d[j] < minn)
			{
				u = j;
				minn = d[j];
			}
		}
		if(u == -1) return -1;
	    vis[u] = true;
	    ans += d[u]; //将与集合s的距离最小的边加入最小生成树
	    for(int v = 0; v < n; v++)
	    {
		   //v未访问&&u能到达v&&以u未中介点可以是v离集合s更近
		   if(vis[v] == false && G[u][v] != inf && G[u][v] < d[v])
		   {
			   d[v] = G[u][v];//将G[u][v]赋给d[v] 
	       }
        } 
	} 
	return ans;//返回最小生成树的边权值和 
} 
```

```c
//邻接表
const int maxv = 510;
const int inf = 0x3fffffff;
struct node{
	int v;
	int dis;
}; 

vector<node>Adj[maxv];
int n;
int d[maxv];//顶点到集合s的最短距离
bool vis[maxv] = {false};

int prim()
{
	int minn;
	fill(d,d + maxv, inf);
	d[0] = 0;
	int ans = 0;
	for(int i = 0; i < n; i++)
	{
		int u = -1, minn = inf;
		for(int j = 0; j < n ; j++)
		{
			if(vis[j] == false && d[j] < minn)
			{
				u = j;
				minn = d[j];
			}
		}
		
		if(u == -1) return -1;
		
		vis[u] = true;
		ans += d[u];
		for(int j = 0; j < Adj[u].size(); j++)
		{
			int v = Adj[u][j].v;
			if(vis[v] == false && Adj[u][j].dis < d[v])
			{
				d[v] = Adj[u][j].dis
			}
		}
	}
	return ans;
} 
```

具体实现

```c
#include <iostream>
#include <algorithm>
#include <cstring>
#include <vector>
#include <cstdio>
#include <string>
#include <cmath>
#include <queue>
using namespace std;
const int maxv = 1000;
const int inf = 0x3fffffff;


int n,m, G[maxv][maxv];
int d[maxv]; //顶点与集合s的最短距离
bool vis[maxv] = {false};
int prim() //默认0号为初始点，函数返回最小生成树的边权之和
{
	int minn;
	fill(d,d + maxv,inf);
	d[0] = 0;
	int ans = 0; //存放最小生成树的边权之和
	for(int i = 0; i < n; i++)
	{
		int u = -1,minn = inf;
		for(int j = 0; j < n; j++)
		{
			if(vis[j] == false && d[j] < inf)
			{
				u = j;
				minn = d[j];
			}
		}
		if(u == -1) return -1;
	    vis[u] = true;
	    ans += d[u]; //将与集合s的距离最小的边加入最小生成树
	    for(int v = 0; v < n; v++)
	    {
		   //v未访问&&u能到达v&&以u未中介点可以是v离集合s更近
		   if(vis[v] == false && G[u][v] != inf && G[u][v] < d[v])
		   {
			   d[v] = G[u][v];//将G[u][v]赋给d[v] 
	       }
        } 
	} 
	return ans;//返回最小生成树的边权值和 
}  
int main()
{
    int u, v, w;
    scanf("%d %d",&n, &m); //顶点，边数 
    fill(G[0],G[0] + maxv*maxv,inf);
	for(int i = 0; i < m; i++)
	{
		scanf("%d %d %d",&u, &v,&w);
		G[u][v] = G[v][u] = w;
	} 
	int ans = prim();
	printf("%d\n",ans);
	return 0;
} 
```

实例：

> 输入：试输入包含若干测试用例。每个测试用例的第1行给出村庄数目N ( < 100 )；随后的N(N-1)/2行对应村庄间的距离，每行给出一对正整数，分别是两个村庄的编号，以及此两村庄间的距离。为简单起见，村庄从1到N编号。
>     当N为0时，输入结束，该用例不被处理。
>
> 输出：  对每个测试用例，在1行里输出最小的公路总长度。

```c
#include <iostream>
#include <vector>
#include <cstdio>
#include <algorithm>
#include <cstring>
using namespace std;
struct node {
	int v;
	int dis;
	node(int x, int y) :v(x), dis(y) { };
};
const int inf = 0x3fffffff;
const int maxv = 1010;
vector<node>adj[maxv];
int n,ans = 0;
bool visit[maxv];
int d[maxv];

int prim()
{
	ans = 0;
	fill(d, d + maxv, inf);
	fill(visit, visit + maxv, false);
	d[1] = 0;
	for (int i = 1; i <= n; i++)
	{
		int u = -1, minn = inf;
		for (int j = 1; j <= n; j++)
		{
			if (visit[j] == false && d[j] < minn)
			{
				minn = d[j];
				u = j;
			}
		}
		if (u == -1) return -1;
		visit[u] = true;
		ans += d[u];
		//cout << "d[u] = " << d[u] <<"u = " << u << endl;
		//cout << "ans = " << ans << endl;
		for (int j = 0; j < adj[u].size(); j++)
		{
			int v = adj[u][j].v;
			int dis = adj[u][j].dis;
			if (visit[v] == false && d[v] > dis)
			{
				d[v] = dis;
			}
		}
	}
	return ans;
}
int main()
{
	while (scanf_s("%d", &n)  && n != 0)
	{
		int x = n * (n - 1) / 2;
		int a, b, len;
		for (int i = 0; i < x; i++)
		{
			cin >> a >> b >> len;
			adj[a].push_back(node(b, len));
			adj[b].push_back(node(a, len));
		}
		int z = prim();
		cout << z << endl;
	}
	return 0;
}
```

> 注意：此两村庄间道路的成本，以及修建状态：1表示已建，0表示未建。若要使修建的话，说明代价为0.就不用再花费金额了。



##### 2.kruskal算法

克鲁斯卡尔算法

> 算法思想；将所有边隐去（所有顶点与顶点之间形成n个连通块）
>
> 1、对所有边从小到大进行排序
>
> 2、按边权从小打大测试所有边，当两个顶点不在同一连通块，则把其加入最小生成树中
>
> 3、执行2、知道出现n-1条边为止
>
> ==通过并查集来查看两个顶点在不在同一连通块==

```c
//大致模板
const int maxv = 1000;
const int inf = 0x3fffffff;

struct edge{
	int u, v;//边的两个端点
	int cost;//边权 
}e[maxv];
bool cmp(edge a, edge b)
{
	return a.cost < b.cost;
} 
int kruskal(int n ,int m) //n为顶点个数，m 为图的边数 
{
	//ans为所求边权之和，num_edge为当前生成树的边数
	int ans = 0, num_edge = 0;
	for(int i = 1; i <= n; i++)
	{
		father[i] = i;
	} 
	sort(e , e + m; cmp);
	for(int i = 0; i < m; i++)
	{
		int fu = findfather(e[i].u );
		int fv = findfather(e[i].v );
		if(fu != fv){
			father[fu] = fv;
			ans += e[i].cost;
			num_edge++;
			if(num_edge == n -1) break;
		}
	}
	if(num_edge != n - 1) return -1;
	else return ans;
} 
```

实例

```c
#include <iostream>
#include <algorithm>
#include <cstring>
#include <vector>
#include <cstdio>
#include <string>
#include <cmath>
#include <queue>
using namespace std;
const int maxv = 1000;
const int inf = 0x3fffffff;
int father[maxv]; //并查集数组 
struct edge{
	int u, v;//边的两个端点
	int cost;//边权 
}e[maxv];
bool cmp(edge a, edge b)
{
	return a.cost < b.cost;
} 

int findfather(int x) 
{
	int a = x;
	while(x != father[x])
	{
		x = father[x];
	} 
	
	while(a != father[a])
	{
		int z = a;
		a = father[a];
		father[z] = x;
	}
	return x;
}

int kruskal(int n ,int m) //n为顶点个数，m 为图的边数 
{
	//ans为所求边权之和，num_edge为当前生成树的边数
	int ans = 0, num_edge = 0;
	for(int i = 1; i <= n; i++)
	{
		father[i] = i;
	} 
	sort(e , e + m, cmp);
	for(int i = 0; i < m; i++)
	{
		int fu = findfather(e[i].u );
		int fv = findfather(e[i].v );
		if(fu != fv){
			father[fu] = fv;
			ans += e[i].cost;
			num_edge++;
			if(num_edge == n -1) break;
		}
	}
	if(num_edge != n - 1) return -1;
	else return ans;
} 
int main()
{
    int n, m;
    scanf("%d %d",&n, &m);
    for(int i = 0; i < m; i++)
    {
    	scanf("%d %d %d",&e[i].u,&e[i].v,&e[i].cost );
	}
	int ans = kruskal(n,m);
	printf("%d",ans);
	return 0;
} 
```

#### 5、拓扑排序

> 拓扑排序是针对==有向无环图==，有向无环图；一个有向图的一个顶点通过一些边无法回到自身的顶点称为有向无环图，可以用来判断一个有向图是否有环
>
> 基本思想；1、定义一个队列q,并把所有入度为0的结点加入队列
>
> 2、取队首结点，输出，然后删去所有从它出发的边，并令这些边到达的顶点入度减一，当这个顶点的入度为0，就把其加入队列
>
> 3、反复进行操作2，知道队列为空，如果入队的数量恰好为N,说明排序成功，否则就不是无环图，而是有环图

```c
//大致模板
//用邻接表实现，但要建立一个数组inDegree[maxv], 只要一开始
//记录号每个顶点的入度就好
vector<int>G[maxv]; // 邻接表
int n, m , inDegree[maxv];

//拓扑排序
bool topologicalsort()
{
	int num = 0;//记录加入拓扑排序的顶点数
	queue <int> q;
	for(int i = 0; i < n; i++) //将所有入度为0的顶点入队
	{
		if(inDegree[i] == 0) q.push(i);
	} 
	while(!q.empty())
	{
		int u = q.front();
		printf("%d ",u);
		q.pop();
		for(int i = 0; i < G[u].size();i++) //遍历该点的所有指向结点
		{
			int v = G[u][i];
			inDegree[v]--;//顶点v入度减一
			if(inDegree[v] == 0) {
				q.push(v);
			} 
		} 
		G[u].clear();
		num++; //入队列的顶点数加一 
	}
	if(num == n) return true; //加入拓扑排序的顶点数恰好为n,是有向无环图 
	else return false; 
} 
```

具体实现

```c
#include <iostream>
#include <vector>
#include <cstdio>
#include <algorithm>
#include <cstring>
#include <queue>
#include <stack>
using namespace std;

int n;
const int maxv = 1010;
vector <int> adj[maxv];
int inDegree[maxv] = { 0 };

void tpsort()
{
	int a[1010] = { 0 };
	int num = 0;
	queue<int>q;
	for (int i = 0; i < n; i++)
	{
		if (inDegree[i] == 0) {
			q.push(i);
		}
	}
	while (!q.empty())
	{
		int u = q.front();
		a[num++] = u;
		q.pop();
		for (int i = 0; i < adj[u].size(); i++)
		{
			int v = adj[u][i];
			inDegree[v]--;
			if (inDegree[v] == 0) {
				q.push(v);
			}
		}
		adj[u].clear();
		//num++;
	}
	if (n == num) {
		for (int i = 0; i < num; i++)
		{
			cout << a[i] << " ";
		}
		cout << endl;
	}
	else {
		printf("ERROR\n");
	}
}
int main()
{
	cin >> n;
	int temp = 0;
	for (int i = 0; i < n; i++)
	{
		for (int j = 0; j < n; j++)
		{
			cin >> temp;
			if (temp)
			{
				adj[i].push_back(j);
				inDegree[j]++;
			}
		}
	}
	tpsort();
	return 0;
}
```



#### 6、关键路径

> AOV网；顶点表示活动，边表示活动的优先关系（拓扑排序就是，顶点表示课程，边的箭头便是活动的优先关系）
>
> AOE网：AOE网表示有向无环图，顶点表示活动的优先关系，边表示活动（有向无环图）
>
> 关键路径：==AOE网的最长路径==，关键路径上的活动成为==关键活动==
>
> 注意点；最长开始时间等于最短开始时间，设立数组e和数组l,分别表示活动ar的最长开始时间和最迟开始时间，当判断e[r] == l[r]是否成立来判断活动r是否为关键活动
>
> 用动态规划算法好理解一些。

```c++
//图先用邻接矩阵存储
//dp[i]计算出来的就是最长路径，当某顶点出度为0，则dp[i]=0,作为顶点（这是逆序拓扑序列的顺序，所以找出度为0点） 
int DP( int i)
{
	if(dp[i] > 0) return dp[i]; //dp[i]一计算得到
	for(int j = 0; j < n; j++) //遍历 i的所有出边 
	{
		if(G[i][j] != inf) {
			dp[i] = max(dp[i], DP(j) + G[i][j]); //找出最长路径 
		} 
    } 
    return dp[i]; //返回计算完毕的dp[i]
} 
```

想要知道最长路径具体为哪一条

> ==设置int型数组choice数组（一个最长路径），保留的时后继结点，因为时逆序拓扑序列==

```c
int DP( int i) //出度为0的点 
{
	if(dp[i] > 0) return dp[i]; //dp[i]一计算得到
	for(int j = 0; j < n; j++) //遍历 i的所有出边 
	{
		if(G[i][j] != inf) {
			int temp = DP(j) + G[i][j]; //单独计算，防止if中调dp函数两次 
			if(temp > dp[i]) { //获得更长路径 
				dp[i] = temp;//覆盖dp[i] 
				choice[i] = j; //i号顶点的后继顶点为j 
			} 
		} 
    } 
    return dp[i]; //返回计算完毕的dp[i]
} 
//调用pirntpath前需要先得到最大的dp[i]，然后将i作为路径的起点传入 
void printpath(int i) 
{
	printf("%d",i);
	while(choice[i] != -1) //choice数组初始化为-1
	{
		i = choice[i];
		printf("->%d",i);
	} 
}
```



### 十八、动态规划

> 定义；将一个复杂问题分解成若干个子问题，通过最优化问题的算法思想，来解决原问题的最优解。动态规划会求解过子问题的解记录下来，这样碰到同样的子问题时，就可以直接利用之前的记录结果，而不是重复计算（再递归中这种方式成为：记忆化搜索）。
>
> 一个问题必须拥有重叠子问题和最优子结构，才能去使用动态规划去解决问题。
>
> 除了递归，其他都需要边界，和状态转移方程

#### 1、动态规划的递归写法

```c
//斐波那契数列（记忆化搜索=动态规划）
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <queue> 
#include <vector>
#include <string>
#include <queue>
#include <cstring> 
using namespace std;
const int maxn = 1000;
int dp[maxn];
int n = 0;
int f(int n)
{
	if(n == 1 || n == 0) return 1; //递归边界 
	if(dp[n] != -1) return dp[n];//已经计算过，直接返回，不在重复计算 
	else{
		dp[n] = f(n - 1) + f(n - 2); //计算f(n),并保存至dp[n] 
		return dp[n];//返回f[n]的结果 
	}
}
int main() 
{
	memset(dp,-1,sizeof(dp));
	cin >> n;
	cout << f(n);
	return 0;
}
```

#### 2、动态规划的递推写法

```c
//数塔问题，每层i有i个数字，先从第一层走到第n层每次只能走向下一层的两个数字中的一个。问最后将路径上的所有数字相加后得到的最大是多少？
//状态转移方程:dp[i][j] = max(dp[i+1][j],dp[i+1][j+1]) + f[i][j];
//边界：dp[n][j] = f[n][j];//从下往上递推
#include <iostream>
#include <algorithm>
using namespace std;
const int maxn = 1000;
int f[maxn][maxn],dp[maxn][maxn];
int main()
{
	int n = 0;
	cin >> n;
	for(int i = 1; i <= n; i++)
	{
		for(int j = 1; j <= i; j ++)
		{
			scanf("%d",&f[i][j]);//输入数塔 
		}
	}
	//边界
	for(int i = 1; i <= n;i++)
	{
		dp[n][i] = f[n][i];	
	} 
	//状态转移方程
	for(int i = n - 1; i >= 1; i--)
	{
		for(int j = 1; j <= i; j++)
		{
			dp[i][j] = max(dp[i+1][j],dp[i+1][j+1])+ f[i][j];
		}
	} 
	printf("%d\n",dp[1][1]);//dp[1][1]就是想要的答案 
	return 0;
}
```

#### 3、最大连续子序列和

> 给定一个数字序列，A1,~An,求i，j使得Ai+…+Aj,最大，输出这个最大和
>
> dp[i]要求必须是以A[i]结尾的连续序列，那么只用两种情况
>
> 1.这个最大的连续序列只有一个元素，以A[i]开始，以A[i]结尾
>
> 2.最大的连续序列有多个元素，从某处A[p]开始，一直到A[i]结尾，这时情况是，当A[i] 与前面的序列相加比自身小，就没必要要了，就是以A[i - 1]结束前面的序列和，A[i]开始了一个新的起点，去开始新的最大连续序列；当A[i]与前面加上设序列和我为Sn，A[p]+…A[i - 1]<  Sn  >A[i];
>
> 针对这两种情况状态转移方程为：`dp[i]=max{A[i],dp[i-1] + A[i]};`边界为首元素dp[0] = A[0];
>
> ==如果让输出最大和中的序列中的最小值和最大值==只需要在弄一个数组，st[0]= a[0],放dp[i - 1] < 0时st[i] = a[i],这时最大序列中的最小值，因为dp[i - 1] < 0,说明序列是从当前序列开始计数的，用st[i]来保持最小值，如果该序列是最大和，那么dp[k] 中的下标k,就可以用a[k]来表示该序列中最大值了
>
> ==如何判断一个序列中最大和序列是不是最大，只需要判断dp[k] < 0,即可，如果大于0，那么有最大和序列，小于0，则没有最大和序列==

```c++
#include <iostream> 0
#include <algorithm>
using namespace std;
const int maxn = 10010;
int A[maxn],dp[maxn];//A[i]存放序列，dp[i]存放以A[i]结尾的连续序列最大和
int main()
{
	int n = 0;
	cin >> n;
	for(int i = 0; i < n; i++)
	{
		scanf("%d",&A[i]);
	}
	//边界
	dp[0] = A[0];
	//状态转移方程
	for(int i = 1; i < n; i++)
	{
		dp[i] = max(A[i],dp[i - 1] + A[i]);
	} 
	//找最大值
	int k = 0;
	for(int i = 1; i < n; i++)
	{
		if(dp[i] > dp[k] ) k = i;
	} 
	cout << dp[k];
	return 0;
} 
```

```c
//找出最大序列和，并输出该序列中的最小值，和最大值,只不过这里的要求是没有最大和序列，那就输出该整序列中的是元素和尾元素，如何判断，
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <string>
#include <cstring>
#include <vector>
using namespace std;
const int maxn = 10010;
int dp[maxn], st[maxn];
int a[maxn];


int main()
{
	int k = 0;
	while (cin >> k && k != 0)
	{
		memset(dp, -1, sizeof(dp));
		memset(st, -1, sizeof(st));
		memset(a, 0, sizeof(a));
		for (int i = 0; i < k; i++)
		{
			cin >> a[i];
		}
		//边界
		dp[0] = a[0];
		st[0] = a[0];
		//状态转移方程
		for (int i = 1; i < k; i++)
		{
			dp[i] = max(dp[i - 1] + a[i], a[i]);
			if (dp[i - 1] < 0) {
				st[i] = a[i];
			}
			else {
				st[i] = st[i - 1];
			}
		}
		int Max = dp[0];
		int index = 0;

		for (int i = 0; i < k; i++)
		{
			if (dp[i] > Max) {
				Max = dp[i];
				index = i;
			}
		}
		if (dp[index] < 0)
		{
			printf("0 %d %d\n", a[0], a[k - 1]);
		}
		else {
			printf("%d %d %d\n", dp[index], st[index], a[index]);
		}
	}

	return 0;
}

```



#### 4、最长不下降子序列（LIS）

> 在一个数字序列中，找到一个最长的子序列（可以不连续），使得这个子序列不是下降的。
>
> 令dp[i]表示以A[i]结尾的最长不下降子序列的长度，有两种情况
>
> 1、如果存在A[i]之前的元素A[j]  (j < i),使得A[j] < =A[i]并且dp[j] + 1> dp[i] (把A[i]跟在以A[j]结尾的LIS后面，能比当前以A[i]结尾的LIS长度更长)，那么就把A[i]跟在以A[j]结尾的LIS后面，形成一条更长的不下降子序列（dp[i] = dp[j] + 1）
>
> 2、如果A[i]之前的元素都比A[i]大，那么A[i]就只好自己形成一条LIS,但是长度为1，即这个子序列里只有一个A[i].所以A[i]结尾的LIS长度就是1，2中能形成的最大长度
>
> dp[i] = max{1,dp[j] + 1}; (j=1,2,…i - 1 && a[j] < a[i])
>
> 边界：dp[i] = 1;//每个数字都先是一个序列

```c
#include <iostream>
#include <algorithm>
using namespace std;
const int N = 100;
int A[N],dp[N];

int main()
{
	int n = 0;
	cin >> n;
	for(int i = 1; i <= n; i++)
	{
		scanf("%d",&A[i]);
	}
	int ans = -1; //记录最大的dp[i]
	for(int i = 1; i <= n; i++)
	{
		dp[i] = 1;//边界初始条件，现假设每个元素自成一个子序列
		 
		for(int j = 1; j < i; j++)
		{
			if(A[j] <= A[i] && dp[j] + 1 > dp[i])
			{
				dp[i] = dp[j] + 1;
			} 
		}
		ans = max(dp[i],ans);
	} 
	printf("%d",ans);
	return 0;
} 
```

#### 5、最长公共子序列（LCS）

> 给定两个字符串，且一个字符串，使得这个字符串是A和B的最长公共部分的长度（子序可以不连续）
>
> dp[i] [j],分别表示A的下标和B的下标
>
> 状态转移方程：当A[i] = B[i]时，dp[i] [j] = dp[i - 1] [j - 1] + 1;
>
> 当A[i] != B[i]时，dp[i] [j] = max(dp[i - 1] [j],dp[i] [j - 1]);
>
> 边界：dp[i] [0]  = dp[0] [j] = 0;
>
> 最终的dp[n] [m]即为答案

```c
#include <iostream>
#include <algorithm>
#include <cstring>
using namespace std;
const int N = 100;
char A[N],B[N];
int dp[N][N];

int main()
{
	int n = 0;
	gets(A + 1);//从下表1开始读入
	gets(B + 1);
	int lenA = strlen(A + 1);//读入是从下标1开始，读取长度也是从+1,开始的 
	int lenB = strlen(B + 1); 
	//边界
	for(int i = 0; i <= lenA; i++)
	{
		dp[i][0] = 0;
	} 
	for(int i = 0; i <= lenB; i++)
	{
		dp[0][i] = 0;
	}
	//状态转移方程
	for(int i = 1; i <= lenA; i++)
	{
		for(int j = 1; j <= lenB; j++)
		{
			if(A[i] == B[j]) {
				dp[i][j] = dp[i - 1][j - 1] + 1;
			} else {
				dp[i][j] = max(dp[i-1][j],dp[i][j - 1]);
			}
		}
	} 
	//dp[lenA][lenB]是答案
    printf("%d\n",dp[lenA][lenB]);
	return 0;
} 
```

#### 6、最长回文子串

> s[i] ~ s[j]是回文字串dp[i] [j] = 1, 不是的话，dp[i] [j] = 0;
>
> 状态转移方程：
>
> dp[i] [j] = dp[i + 1] [j - 1], (s[i] == s[j]);
>
> dp[i] [j] = 0,(s[i] != s[j])
>
> 边界
>
> dp[i] [i] = 1,dp[i] [i + 1] == (s[i] == s[i + 1])?1 :0;
>
> 大致思路；一个字符肯定是是回文子串，作为边界，另一个边界时当有两个字符是，判断两个字符是否，一样，一样的话，就是回文字串。当长度为3时，就用状态转移方程就好

```c++
#include <iostream>
#include <algorithm>
#include <cstring>
using namespace std;
const int maxn = 1010;

char s[maxn];
int dp[maxn][maxn];

int main()
{
    gets(s);
    int len = strlen(s);
    int ans = 1; //记录最大回文串的长度
	memset(dp,0,sizeof(dp));//dp数组初始化为0
	//边界
	for(int i = 0; i < len; i++)
	{
		dp[i][i] = 1; 
		if(i < len - 1)
		{
			if(s[i] == s[i + 1])
			{
				dp[i][i+1] = 1;
				ans = 2;
			}
		}
	} 
	//状态转移方程
	for(int L = 3; L <= len; L++) //枚举字串的长度 
	{
		for(int i = 0; i + L - 1 < len; i++) //枚举字串的起始端点
		{
			int j = i + L - 1;//字串的右端点
			if(s[i] == s[j] && dp[i + 1][j - 1] == 1)
			{
				dp[i][j] = 1;
				ans = L;//更新最长回文子串的长度 
			} 
		} 
	} 
	printf("%d\n",ans); 
	return 0;
} 
```

实例；有一行字符串，值判断字符是否为回文字符（其它的空格，非数字，字母字符忽略不计），最后输出的确实含这些字符的回文字符串

```c
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <string>
#include <cstring>
#include <vector>
#include <cctype>
using namespace std;
const int maxn = 5010;
char a[maxn], b[maxn];
int p[maxn];
int dp[maxn][maxn];

int main()
{
	memset(dp, 0, sizeof(dp));
	cin.getline(a, 5000);
	int lena = strlen(a);
	int k = 0;
	for (int i = 0; i < lena; i++)
	{
		if (isalnum(a[i])) {
			//cout << "a[i] = " << a[i] << endl;
			p[k] = i;
			if (a[i] >= 97 && a[i] <= 122) {
				b[k] = a[i] - 32;
			}
			else {
				b[k] = a[i];
			}
			k++;
		}
	}
	int start = 0;

	int ans = 1; //记录回文串长度
	//边界
	for (int i = 0; i < k; i++)
	{
		dp[i][i] = 1;//初始都为1
		if (i + 1 < k) {
			if (b[i] == b[i + 1]) {
				dp[i][i + 1] = 1;
				ans = 2;
			}
		}
	}
	//状态转移方程
	for (int L = 3; L <= k; L++) //回文串的长度
	{
		for (int i = 0; i + L - 1 < k; i++)
		{
			int j = i + L - 1;
			if (b[i] == b[j] && dp[i + 1][j - 1] == 1)
			{
				dp[i][j] = 1;
				start = i;//最左侧
				ans = L;
			}
		}
	}
	for (int i = p[start]; i <= p[start + ans - 1]; i++)
	{
		cout << a[i];
	}

	return 0;
}

```



#### 7、DAG最长路

略

#### 8、01背包问题

> 问题描述：有n件物品，每件物品重量为w[i],价值为c[i],现有一个容器为v的背包，问如何选取物品放入背包，使得背包内物品总价值最大，其中每种物品都只有1件。
>
> dp[i] [v]表示前i件物品恰好装入容量为v的背包中所能获得的最大价值。求dp[i] [v]的方式
>
> 1、不放第i件物品，那么问题转换为前i - 1件物品恰好装入容量为v的背包中所能获得的最大价值，也即dp[i -1] [v]
>
> 2、放第i件物品，那么问题转换为前i - 1件物品恰好装入容量为v - w[i]的背包中所能获得的最大价值,也即dp[i - 1] [v - w[i]] + c[i]
>
> 状态转移方程：dp[i] [j] = max{dp[i - 1] [v], dp[i - 1] [v - w[i]] + c[i]};
>
> 边界：for(int v = 0; v < n; v++) dp[0] [v] = 0;

```c
for(int i = 1; i <= n; i++)
{
    for(int v = w[i]; v < V; v++)
    {
        dp[i][j] = max(dp[i - 1][v],dp[i - 1][v - w[i]] + c[i]);
    }
}
```

> 以上方法不够优化，降低空间复杂度
>
> dp[v] = max(dp[v],dp[v - w[i]] + c[i]); //不过v是逆序输出,这是因为遍历的是i 的上发i-1,和i的左上方，v = w[i] ~V
>
> 边界为dp[v] = 0; v=0 ~ V;

```c
for(int i = 1; i <= n; i++)
{
    for(int v = V; v >= w[i]; v--){
        dp[v] = max(dp[v],dp[v - w[i]] + c[i]);
    }
}
```

```c++
//优化模板
#include <iostream>
#include <algorithm>
#include <cstring>
using namespace std;
const int maxn = 100; //物品最大件数 
const int maxv = 1000; //V的上限 
int w[maxn], c[maxn] ,dp[maxv]; 
int main()
{
    int n, V;
    scanf("%d %d",&n,&V);
    for(int i = 0; i < n; i++)
    {
    	scanf("%d",&w[i]);
	}
	for(int i = 0; i < n; i++)
	{
		scanf("%d",&c[i]);
	}
	//边界
	for(int v = 0; v <= V; v++)
	{
		dp[v] = 0;
	} 
	//状态转移方程
	for(int i = 1; i <= n; i++)
	{
		for(int v = V; v >= w[i];v--)
		{
			dp[v] = max(dp[v],dp[v - w[i]] + c[i]);
		}
	} 
	int Max = 0;
	for(int v = 0; v <= V; v++)
	{
		if(dp[v] > Max) Max = dp[v];
	}
	printf("%d\n",Max);
	return 0;
} 
```

> 这样类问题也是属于动态规划问题；容量体积为V,数量有n,且n个数，每个数都有自己的容量，现在让自己的背包容量最小；
>
> 状态转移方程:**dp[v]=min(dp[v],dp[v-v[i]]-v[i])**,其中v[i]表示每件物品的体积
>
> dp[i]的含义:箱子剩余的最小空间,
>
> min的第一个参数代表:不放入第i件物品时，箱子剩余的最小空间
>
> 第二个参数代表:放入第i件物品时,箱子剩余的最小空间,
>
> 为什么会有两个-v[i]呢?
>
> 原因是:第一个 -v[i] 代表 放入v[i]后箱子的容量(必须大于等于0),第二个则是放入v[i]之后箱子的剩余容量
>
> ==边界；dp[i] = V;初始化总体积，因为要从总体积去减==

```c
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <string>
#include <cstring>
#include <vector>
#include <cctype>
using namespace std;
const int maxn = 20010;
int dp[maxn];
int v[maxn];
int main()
{
	int V, n;
	cin >> V >> n;
	for (int i = 0; i < n; i++)
	{
		cin >> v[i];
	}
	//边界
	for (int i = 0; i <= V; i++)
	{
		dp[i] = V; //剩余总空间
	}
	//状态转移方程
	for (int i = 0; i < n; i++) //数量
	{
		for (int j = V; j >= v[i]; j--) //剩余空间
		{
			dp[j] = min(dp[j], dp[j - v[i]] - v[i]);
		}
	}
	int min_space = V;
	for (int i = 0; i <= V; i++)
	{
		if (dp[i] < min_space) {
			min_space = dp[i];
		}
	}
	cout << min_space;
	return 0;
}

```



#### 9、完全背包问题

> 问题描述：有n件物品，每件物品重量为w[i],价值为c[i],现有一个容器为V的背包，问如何选取物品放入背包，使得背包内物品总价值最大，其中每种物品有无穷件。
>
> dp[i] [v]表示前i件物品恰好装入容量为v的背包中所能获得的最大价值。求dp[i] [v]的方式
>
> 1、不放第i件物品，那么问题转换为前i - 1件物品恰好装入容量为v的背包中所能获得的最大价值，也即dp[i -1] [v]
>
> 2、放第i件物品，那么问题转换为第 i 件物品恰好装入容量为v - w[i]的背包中所能获得的最大价值,也即dp[i ] [v - w[i]] + c[i]，==这是因为选则放第i件物品并不意味着必须转移到dp[i - 1] [v - w[i]]这个状态，而是转移到dp[i] [v - w[i]]这个状态，因为每件物品有任意件，选了第i件，还可以接着选第i件物品==。
>
> 状态转移方程：dp[i] [j] = max{dp[i - 1] [v], dp[i ] [v - w[i]] + c[i]};
>
> 边界：for(int v = 0; v < n; v++) dp[0] [v] = 0;

```c
for(int i = 1; i <= n; i++)
{
    for(int v = w[i]; v < V; v++)
    {
        dp[i][j] = max(dp[i - 1][v],dp[i][v - w[i]] + c[i]);
    }
}
```

> 以上方法不够优化，降低空间复杂度
>
> dp[v] = max(dp[v],dp[v - w[i]] + c[i]); //不过v是正序输出,这是因为遍历的是i 的上发i-1,和i的左方，v = w[i] ~V
>
> 边界为dp[v] = 0; v=0 ~ V;

```c
for(int i = 1; i <= n; i++)
{
    for(int v = w[i];v <= V; v++){
        dp[v] = max(dp[v],dp[v - w[i]] + c[i]);
    }
}
```

```c
//优化模板
#include <iostream>
#include <algorithm>
#include <cstring>
using namespace std;
const int maxn = 100; //物品最大件数 
const int maxv = 1000; //V的上限 
int w[maxn], c[maxn] ,dp[maxv]; 
int main()
{
    int n, V;
    scanf("%d %d",&n,&V);//数量， 背包容量
    for(int i = 0; i < n; i++)
    {
    	scanf("%d",&w[i]);//每类数量中的容量大小
	}
	for(int i = 0; i < n; i++)
	{
		scanf("%d",&c[i]);//价值
	}
	//边界
	for(int v = 0; v <= V; v++)
	{
		dp[v] = 0; //背包的容量从0到最大值都是0
	} 
	//状态转移方程
	for(int i = 1; i <= n; i++) //数量
	{
		for(int v = w[i]; v < V;v++) //容量
		{
			dp[v] = max(dp[v],dp[v - w[i]] + c[i]);
		}
	} 
	int Max = 0;
	for(int v = 0; v <= V; v++)
	{
		if(dp[v] > Max) Max = dp[v];
	}
	printf("%d\n",Max);
	return 0;
} 
```

实例

> 货币系统中货币的种类数目是 V 。 (1<= V<=25)
> 要构造的数量钱是 N 。 (1<= N<=10,000)
>
> 钱数代表背包总大小
>
> 每种货币的面值看作商品的价值，状态转移方程是dp [ i ] [ v ] = dp [ i - 1 ] [ v ] + dp [ i - 1 ] [ v-v[ i ] ] ，dp[ i ] [ v ] 代表当前能构造出 钱数v的方案数，初始化时 dp[ 0 ] =1 ，其余均为 0

```c
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <string>
#include <cstring>
#include <vector>
#include <cctype>
using namespace std;
const int maxn = 20010;
long long dp[maxn];
long long m[maxn];
int main()
{
	long long V, N;
	
	while (cin >> V >> N)
	{
		for (int i = 0; i < V; i++)
		{
			cin >> m[i]; //每个货币的面值
		}
		fill(dp, dp + maxn, 0);
		dp[0] = 1;//目前有一种
		for (long long i = 0; i < V; i++)
		{
			for (long long v = m[i]; v <= N; v++)
			{
				dp[v] = dp[v] + dp[v - m[i]];
			}
		}
		cout << dp[N] << endl;
	}
	return 0;
}

```









### 十九、find函数匹配字符串的使用

实例一：利用find中看字符串是否有此字串

```c
#include <iostream>
#include <cstdio>
#include <cstring>
#include <string>
#include <algorithm>
#include <vector> 
using namespace std;

//chi1 huo3 guo1
int main()
{
	string a;
	int ans = 0;
	int index = 0;
	int count = 0;
	int flag = 0;
	while (getline(cin, a) && a != ".")
	{
		ans++;
		if (a.find("chi1 huo3 guo1") != string::npos)
		{
			count++;
			if (flag == 0) {
				index = ans;
				flag = 1;
			}
		}
	}
	cout << ans << endl;
	if (count == 0) printf("-_-#");
	else printf("%d %d", index, count);
	return 0;
}
```

实例二、利用find函数看字符串中有多少个字串

```c
#include <iostream>
#include <cstdio>
#include <string>
#include <cstring>
#include <cmath>
using namespace std;


int main()
{
    string a, b;
	int t = 0;
	cin >> t;
	int m, n;
	while(t--)
	{
		cin >> m >> n;
		cin >> a;
		cin >> b;
		for(int i = 0; i < a.size() ;i++)
		{
			if(a[i] >=  'A' && a[i] <= 'Z') a[i] += 32;
		}
		for(int i = 0; i <b.size() ;i++)
		{
			if(b[i] >=  'A' && b[i] <= 'Z') b[i] += 32;
		}
		int x = 0;
		int count = 0;
		for(int j = 0; j < b.size(); j++)
		{
			if(b.find(a,j) == j){
				count++;
			}
		}
		cout << count << endl;
	} 
	return 0;
}

//表示从位置j除去匹配字符串a,如果下表为j的话，表示当前位置就有一个该字串，然后从下一个位置开始遍历
```

find() 、replace()、erase（）函数大用法

```c++

```

