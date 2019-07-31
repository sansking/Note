###WebGL:
    在浏览器上流畅展示3D模型和场景的一种技术;使用js作为编程语言,调用浏览器支持的3D绘图函数,来实现3D模型和场景的展现;
    浏览器实现了openGL ES的规范;这套规范可以直接使用指令操作显卡,使显卡渲染到3D世界,直接反应到浏览器上;
###Three.js:
    四大组件: 场景,相机(透视相机(体现远近关系),正投影相机),渲染器,几何体
        THREE.Scene = function();
        THREE.PerspectiveCamera = function(fov,aspect,near,far);
            //参数:fov,视角,代表下平面和上平面的夹角,单位默认为degree
                aspect,横纵比,实际窗口的宽高比例
                near,离近平面的距离
                far,离远平面的距离
            let camera = new THREE.PerspectiveCamera(45,height/width,1,1000);
        THREE.WebGLRenderer = function({初始化参数});
###Three.js的绘图步骤:
    1.定义视图&相机&渲染器
        // 初始化其他配置,包括渲染器的长度和高度,颜色,以及相机的位置
        // 将渲染器加入到某个html标签中,使其成为一个dom元素
    2.定义几何体,材质,然后组成网格
    3.将网格模型加入scene中
    4.定义渲染函数
        // 在该函数中,需要使用 requestAnimationFrame(self);对每一帧进行渲染

    疑问:
        场景,相机,几何体 默认位置都是0,0,0
        Three.js在后期版本中,没有 setClearColorHex 方法
###Three.js程序框架,绘制一条直线:
    //可以在body的onload事件中启动主函数
    // 初始化时,需要初始化以上四种元素,还有灯光等
###三维世界的组成:
    点,线,面;
    点的定义:THREE.Vector3 = function(x,y,z);默认值为0,0,0;
    几何体的定义:
        点: this.vertices = [],
        颜色: this.colors = [],
        面: this.faces = [];
    如: 以下定义一个几何体,有两个点,每个点对应的颜色如下
        let geometry = new THREE.Geometry();
        geometry.vertices.push(new THREE.Vector(0,10,0),new THREE.Vector(0,10,0));
        geometry.colors.push(new THREE.Color(0xffffff),new THREE.Color(0x00ff00));
    材质: 表面各个可视属性的结合,包括 色彩,纹理,光滑度,透明度,反射率,折射率,发光度等;
        THREE.LineBasicMaterial = function(parameters);
            parameters 为一个对象,有如下一些属性:
                Linewidth,Linecap,Linejoin,VertexColors(true/false)
                // WebGLRenderer不支持线的宽度,CanvasRenderer支持
        颜色插值:
            插值由拟合函数决定;
        线的绘制方式:
            两点,循环等
###坐标系(世界坐标,本地坐标):
    左手坐标系: z轴向里
    右手坐标系: z轴向外(webgl和three.js使用右手坐标系)
    // 一般,红绿蓝分别代表 x,y,z 轴
####围绕某个轴进行旋转:
    右手大拇指指向轴的方向,其他四指自然弯曲,指向的方向就是旋转的正方向;
    旋转时,某个轴的值 +2*pi 即旋转一周,即大约为6.28代表一周;
    旋转可以以本地坐标系旋转,也可以围绕世界坐标系旋转;
    可以使用 let axis = new THREE.AxisHelper(n); scene.add(axis);
    // 以下方式可以将其加入到某个obj中
    let allObj = new THREE.Object3D();
    allObj.add(mesh1); allObj.add(mesh2);
    scene.add(allObj);
###Three.js让场景动起来的两种方式:
    1.变化物体的位置
    2.变化相机的位置
    渲染循环:
        function animate(){
            renderer();
            requestAnimationFrame(animate);
        }
    fps(frames per second):每秒帧数
    js性能监控的插件(stats.js)
        let status = new Status();
        status.setMode(1);  //0:fps,1:ms,2:mb
        status.domElement.style.position = "absolute";
        status.domElement.style.left = "0px";
        document.body.appendChild(status);
        // 然后在循环中使用: status.begin();status.end();
        tween.js: 用于实现渐变效果
###游戏循环(帧循环,渲染循环)
    一般的游戏循环
    while(true){
        update_status();
        draw();
        // 一般会先清空当前画面,然后再绘制新的场景
        // 电影中由于有胶片的曝光延迟,动作存在残影,因此即使帧率不是很高,也可以有较高的流畅度;
    }
###相机的工作原理
    正投影相机(平行光投影(包括正投影和斜投影))
    透视投影相机(点光源投影)
    OrthograhicCamera = function(left,right,top,bottom,near,far);
    投影时,显示的画面都是投影到近平面的画面;
    相机的位置:
        position属性,position.x,y,z
        lootAt属性,代表相机镜头的方向,该方向为相机位置,和lootAt的点所连接的直线
        up属性,该属性对应的点,和原点连接而成的直线,代表了相机的上方向;
        // 以上三个属性决定了相机的绝对位置
        注: lookAt属性对应的向量,和up属性对应的向量 必须是[垂直]的
####dat.gui.js控件
    视角越大,看到的近平面就越大,因此,投影在近平面上的内容显得越小;
    视角越小,看到的近平面就越小,因此,投影在近平面上的内容就显得越大;
###光的初体验
    THREE.Light = function(color);
    没有任何光的情况下,任何物体都是黑色;
    在定义material时,可以传入一个颜色,代表该材质本身的颜色(当白色光的情况下,显示的颜色)
####环境光: 
        let lignt = new THREE.AmbientLight(0xffff00);
        环境光的位置没有意义
    光的过滤:
        在图形学中,绿色的光照在红色物体上,红色物体不会反射任何光;
        照射光中,如果没有红色的分量,则不会显示任何光;
####方向光:
    平行光又称为方向光(DiirectionalLight),是一组没有衰减的平行的光线;
    DirectionalLight(hex,intensity);(分别代表 颜色与光强(0(黑色)~1));
    方向光不能在物体里面;否则无法照亮物体
    默认情况下,光会穿透物体
####点光源:
    从一点发出的光;向四周发出的光
    PointLight(hex,intensity,distance,decay);
        // decay代表在distance范围内,衰减的比率
###纹理:
    纹理坐标:x,y轴以0~1的浮点数表示;
    该纹理图片上的每一个点对应某个特定的坐标;
    如果纹理图片绝对大小和网格的设置不匹配,则会产生相应的伸缩
    // 纹理坐标代表的是图片和模型的对应关系
    THREE.Texture(img,mapping,wrapS,wrapT,magFilter,
        minFilter,format,type,anisotropy);
        img:图片类型,由ImageUtils加载
            THREE.TextureLoader: 该类用于加载文件
        // 关于文件跨域问题: 可以通过配置google浏览器的启动参数以使其可以读取本地文件
        加载的图片应该是2的n次方;如果加载的图片宽和高不是2的n次方,显卡会将其伸缩为最接近的大小
    let geometry = new THREE.PlaneGeometry(width,height);
    let material;
    textureLoader = new THREE.TextureLoader();
    textureLoader.load(url,onLoad,onProgress,onError);
        // 异步加载的回调函数,url(也可以是本地的相对路径)以及三个回调
        onLoad对应的函数默认参数为 texture;
        onProgress和onError函数默认参数为 xhr,即对应的ajax请求;
    因此,加入mesh的时机为onLoad方法内;
    loader.load(url,function(texture){
        let material = new THREE.MeshBasicMaterial({
            map:texture,
        });
        mesh = new THREE.Mesh(geometry,material);
        scene.add(mesh);
    });
####纹理的重复与纹理的回环:
    纹理的重复由材质的属性 repeat 决定;
        texture.repeat.x/y;
        // 纹理的重复可以是小数(相当于截断),负数(相当于旋转)
    纹理的回环有材质的属性 wrap 决定;
        texture.wrapS/T = THREE.RepeatWrapping;
        wrapS/T :分别代表x/y轴的回环
        (回环有三种方式: RepeatWrapping,ClampToEdgeWrapping,MirrorRepeatWrapping)
        Repeat为简单重复,ClampToEdge为边缘拉伸,Mirror为镜像(包括左右和上下镜像)
        // 分别对应 1000,1001,1002;
    纹理被缓存在显存中,如果不调用 texture.needUpdate = true;则纹理不会被更新,因此对WrapS/T的修改不会显示出来
####绘制彩色的三角形:
    let geometry = new THREE.PlanGeometry(100,100,1,1);
        // 最后两个参数实际上代表重复次数
    let material = new THREE.MeshBasicMaterial({vertexColors:THREE.VertexColors,color:0xff0000,wireFrame:true});
        // 注: vertexColors赋值可以为没有颜色,使用面的颜色,使用定点的颜色
    let mesh = new THREE.Mesh(geometry,material);
    scene.add(mesh);
    // 注: 在三维设计中,为了避免在不同面绘制颜色,使用两个三角形来代替四边形,因为三个点一定在同一个平面上,而四个点不一定;
    Geometry的faces属性可以看出该几何体的所有平面的绘制方式,同时,也可以对faces中某个元素的顶点的颜色进行赋值;
    由于取顶点的颜色,因此material的构造函数中不能定义颜色;

####Geometry的结构:
    vertices,faces,colors;代表顶点,面,颜色;
    //  注:如果要自定义Geometry时,需要自定义面
    let face = new THREE.Face3(0,1,2); // 使用顶点的索引作为参数(vertices数组中的索引)

###三维模型的加载与显示基础:
    三维模型:存放三维数据的文件,叫做模型文件;
    模型文件的存储结构: 点,线,面;每个面不同的颜色和纹理,每个面不同的光照效果(法向量);
    模型查看器: ParaView,Blender(类似于3DMax)
    学习用的模型下载网站: http://ww.cc.gatech.edu/projects/large_models/
    vtk模型:
        VTKLoader.js    // 用于加载模型文档
        Detector.js     // 用于检查浏览器是否支持webGL
            var canvas = document.createElement('canvas');
            return !!(window.WebGLRenderingContext && 
            (canvas.getContext('webgl')||canvas.getContext('experimental-webgl')));
        
        // 使用以下方法来检验是否支持webgl
        if(!Detector.webgl) Detector.addGetWebGLMessage();
        let dirLight = new THREE.DirectionLight(0xffffff);  //定义白光
        dirLight.position.set(200,100,300);

    
        camera.add(dirLight);
        camera.add(dirLight.target);
        let material = new THREE.MeshLambertMaterial({color:0xffffff,side:THREE.DoubleSide});
        THREE.FrontSide/BackSide/DoubleSide(双面绘制,因此效率会降低)
        // FrontSide和BackSide实际上可以认为是外面和里面,而不是前面和外面;
        // 绘制的FrontSide和BackSide与faces[i]中,顶点的顺序有关;
        // 然而,由于光的穿透,即使设置为BackSide,在外部的camera也可以看到某个物体,但是看到的是另一侧的内部;
        let loader = new THREE.VTKLoader();
        loader.load('bunny.vtk',function(geometry){
            geometry.computeVertexNormals();    // 计算法向量,以实现光的折射和反射;
            mesh.position.setY();   // 可以通过该方法来改变mesh的位置
        });
        angle += 0.1;
        mesh.rotateY(angle);

###VTKLoader的内部实现原理
    VTKLoader
        onProgress(loaded,total);   // 代表过程中已加载的容量,以及总量;
        scope.parse(text);  //将text文本转换为geometry对象;
        POLYGONS: 代表多边形;
        // js中,正则可以使用exec来得到匹配的结果,如果 该结果不为null,则说明存在匹配;
####THREE.BufferGeometry
    将数据放在一块[连续]的内存引用中;因此,可以节省传递数据到GPU的时间;
    BufferGeometry也包含顶点,法线,位置,索引等内容;
    BufferAttribute(array,itemSize)与BufferGeometry联用,存放属性数据
###WebGL性能:
    高效的渲染几何体,如何保持高帧数:
        //数据结构的优化
        渲染160万三角形,还能达到35帧以上;
        约束:三角形的大小,三角形在某个立方体内;
        (使用BufferGeometry)
        预备:(number = 1600000);
            1.顶点: position (数组长度:new Float32Array(number*3*3));   //3个顶点,每个顶点具有x,y,z坐标
            2.法线: normal 
            3.颜色: color
            // 对于顶点的数组,需要循环number次,每次都需要计算出3个顶点的x,y,z轴数据
            geometry.computeBoundingSphere();   // 计算包围盒的大小
            // 对于矩形: 计算x,y,z的最小最大值
            // 对于球体: 计算中心点和r
            // 一般用于碰撞检测
    高效绘制点数据(粒子效果):
        PointMaterials({size:3,})   //点变大对性能影响不大
            // 点材质,主要和粒子系统一起使用
        points = THREE.Points(geometry,materials);
        scene.add(points);  // 可以将粒子系统加入到scene中
        scene.fog = new THREE.Fog(color,near,far);  //雾效
        点的数量设为number,则点的positions为 new Float32Array(number*3);
###WebGL模型
####Obj模型(文本格式):
#####加载Obj模型
    let manager = new THREE.LoaderManager();
    texture.image = image;
    let objLoder = OBJLoader(manager);
    objLoader.load(url,onloaded(obj){
        // 此处的obj为一个 Object3D对象;
        // traverse方法用于遍历该Object3D对象
        obj.traverse(function(child){
            if(child instanceof THREE.Mesh){
                child.material.map = texture;
            }
        })
    },onProgress(e){
        if(e) {let hasLoad = e.loaded/e.total * 100;}
    });
#####Obj模型的格式
    正则解析文本
###多视图效果
    多个相机,多个渲染器,单个场景
    // 如果把单个渲染器赋给多个div,则会导致其dom元素加入到最后一个div中;
    // 如果把多个渲染器的dom元素赋值给单个div,则会导致其中某个canvas被挤压;
    // 如果使用绝对定位,也需要设置渲染器为透明,才能实现页面元素的叠加
    多个相机,多个渲染器,多scene
###通过鼠标拾取物体
    通过鼠标选中某个物体,称为拾取;
    鼠标相对于canvas/渲染器左上角的位置
    Raycaster = function(origin,direction,near,far);
        // 参数分别为: 原点,方向,近平面的距离,远平面的距离
        // 相机位置所在点,和鼠标所在点,所连成的射线,穿过的物体被视为拾取;
    var mouse = THREE.Vector2D();
    THREE.BoxGeometry = function(width,height,depth,widthSegments,heightSegments,depthSegments);
    {color:Math.random * 0xffffff}; // 随机颜色
    camera.updateMatrixWorld();

    setFromCamera:function(coords,camera);
        // coords:鼠标的位置,归一化的设备坐标,必须为-1到1之间
        // camera:某个相机
        // 浏览器坐标: 左上角为0,0
        // 设备归一化坐标: 以设备中心点为原点的坐标系,与世界坐标系很像;
        // 该方法用于计算一条raycaster,其方向为相机指向鼠标的坐标;
        raycaster.intersectObjects(scene.children);
        // 该方法用于找到一个被拾取的物体,返回值为一个mesh数组
###物体的旋转及技巧
    平移,旋转,缩放
    GridHelper(size,step);  //该类用于绘制网格;
    例如GridHelper(1000,50); 会绘制200*200个小平面;
    由于Mesh派生于Object3D,因此可以直接通过Mesh的旋转(rotateX等)来进行旋转等操作;
        // 围绕着当前对象的中心(position)进行旋转,可以直接通过object3D的rotateX/Y/Z进行旋转
        // 如果希望当前对象围绕着另外一个点进行旋转,则可以将该物体围绕和
        THREE.Group();派生于Object3D,可以通过add方法向组中加入对象;
        此时,group的中心点和相关对象的中心点重合
        然后,通过改变原对象的xyz偏移,以及group对象的xyz偏移;
        然后旋转group进行旋转,就可以改变原本对象的旋转方式
        rotateOnAxis(Vector3,angle) 可以围绕任意轴旋转,每次旋转angle度;
        THREE.AxisHelper(length);    // length为该轴的长度
        THREE.BoxHelper(mesh) 得到某个对象的包围盒对象,该对象为一个mesh;
        box = THREE.Box3().setFromObject(mesh);
            // box得到的是最大最小值
        box.center(obj.position); 
        multiplyScale() 该方法可以对某个对象进行缩放;
###粒子系统
    粒子特点:
        1.粒子非常小
        2.粒子非常多
        3.粒子有一定的运动规律
    实现思路:
        1.从一个人物的3D模型中,取得点,由这些点构成粒子系统
        2.为粒子染色
        3.让粒子动起来
        4.让场景旋转起来
    加载jSON模型:
        JSONLoader: JSON模型加载器
        可以直接使用 new THREE.JSONLoader()得到Json加载器
    缩放模型:
        mesh.scale.x = n; n代表倍数;
        加载meterial时,可以使用
        new THREE.MeshBasicMaterial({
            color:0xfffff,wireframe:true
        });
    由Geometry生成粒子系统
        THREE.Points = function(geometry,material);
        结合geometry和material得到粒子系统;
    THREE.Clock(); 通过该类查看时间

    // 实际上是移动每一个顶点;因此需要遍历整个顶点数组
    // 如果是向上移动,则需要使用粒子现在的位置渐进移动到粒子原本的位置;
###游戏
    游戏主要元素:
        地图,碰撞,模型,移动,摄像
    游戏整体比例:
        天空盒: 5000*5000*5000
        摄像机: 至少看1米,最多看10000米
        地图: 宽度12个方块,每个方块大小为10
        楼房宽度: 10
    游戏架构:
        1.游戏类: 负责游戏的初始化,开始和结束
        2.地图类: Maps
        3.飞行器类: Navigation
    游戏状态机:
        游戏开始--游戏运行--游戏暂停--游戏结束
        // 1.初始化方法:
            a.初始化renderer,scene,camera,light等
            b.初始化其他资源,声音,模型等
                // 注:天空盒由6幅图组成,贴在立方体上以执行渲染方法
        地图数组:
            a.mapString: 地图信息,一个2位矩阵,用于保存地图的位置信息
####Tiled
    地图编辑器
###贝塞尔曲线
    自定义形状:
        THREE.Shape(接收点的参数)
        THREE.Line(绘制线)
    