# Boost
---

## 安装

1. 执行 bootstrap.bat 或 bootstrap.sh 脚本.
2. 执行 bjam.exe 编译boost库

## 测试公共代码

```
namespace bg = boost::geometry;  
typedef bg::model::d2::point_xy<double> DPoint;  
typedef bg::model::segment<DPoint> DSegment;  
typedef bg::model::linestring<DPoint> DLineString;  
typedef bg::model::box<DPoint> DBox;  
//这里的ring就是我们通常说的多边形闭合区域(内部不存在缕空)，模板参数为true，表示顺时针存储点，为false，表示逆时针存储点，由于MM_TEXT坐标系与传统上的坐标系的Y轴方向是相反的，所以最后为false，将TopLeft、TopRight、BottomRight、BottomLeft、TopLeft以此存储到ring中，以便能正确计算  
typedef bg::model::ring<DPoint, false> DRing;  
//polygon模板参数false，也是由上面相同的原因得出来的  
typedef bg::model::polygon<DPoint, false> DPolygon; 


```

## Geometry 模块

1. 计算两点之间的距离

	```
	DPoint pt0(100, 100);  
	DPoint pt1(200, 200);  
	double dDistance = bg::distance(pt0, pt1);  
	```
1. 点到线段的垂直距离（可能在延长线上）

	```
	DPoint pt0(100, 100);  
	DPoint pt1(200, 200);  
	DSegment sg0(pt0, pt1);
	double dDistance = bg::distance(DPoint(200, 100), sg0);  
	```

1. 判断线段是否相交
	
	```
	DSegment sg1(DPoint(0, 100), DPoint(100, 0));  
    DSegment sg2(DPoint(100, 200), DPoint(200, 100));  
    bool bIntersect = false;  
    bIntersect = bg::intersects(sg0, sg1); 
	```

1. 计算线段和线段的交点

	```
	
	```