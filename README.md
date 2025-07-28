# xAnime

### lvgl 动画插件，简化动画配置，直观高效，多对象应用动画

# 使用方式

## 引入头文件

```c
include "xanime.h"
```

## 创建动画

有两种创建方式

- 没有返回值，创建并自动播放动画
- 有返回值，需要手动播放，并在之后手动销毁

### 没有返回值

可以配置单对象，或者多对象动画

#### 单对象动画

```c
lv_obj_t *img2 = lv_img_create(lv_scr_act());
lv_img_set_src(img2, &to_2);
lv_obj_center(img2);

// 创建单对象动画
xanime_create_single(img2, (xanime_param_t){
    // 下面的参数配置可以参考下文的结构体定义
    .x = "50%",
    .y = "50%",
    .dur = "5000",
    .rotate = "3600",
    .loop = "-1"
});
```

#### 多对象动画

```c
lv_obj_t *btn_test = lv_btn_create(lv_scr_act());
lv_obj_align(btn_test, LV_ALIGN_CENTER, 0, 0);
lv_obj_t *btn_test1 = lv_btn_create(lv_scr_act());
lv_obj_align(btn_test1, LV_ALIGN_RIGHT_MID, -10, 0);

// 创建多对象数组
lv_obj_t *obj_arr[] = {btn_test, btn_test1};

xanime_create(
    (xanime_obj_t){
        // 配置对象数量
        .obj_num = 2,
        // 引用对象数组
        .obj_arr = obj_arr,
    },
    (xanime_param_t){
        .x = "50",
        .y = "50%",
        .rotate = "3600",
        .dur = "5000",
        .loop = "-1",
    });
```

### 有返回值

```c
xanime* anime = xanime_create_single_rt(obj, (xanime_param_t){
        ...
    })
xanime_start(anime);
xanime_delete(anime);
```

## 动画参数

#### 动画参数结构体

```c
// 动画参数结构
typedef struct {
  // X 位置 (INT32_MIN 表示不设置)
  char* x;
  // Y 位置
  char* y;
  // 宽度
  char* width;
  // 高度
  char* height;
  // 透明度 (0-255)
  char* opacity;
  // 旋转角度 (0-360)
  char* rotate;
  // 缩放
  char* scale;
  // 持续时间 (ms)
  char* dur;
  // 延迟时间 (ms)
  char* delay;
  // 循环次数 (0=不循环, -1=无限循环)
  char* loop;
  // 旋转中心 x
  char* pivot_x;
  // 旋转中心 y
  char* pivot_y;
  // 自动播放
  bool auto_play;
  // 缓动函数
  xanime_easing_t easing;
  // 动画完成回调
  lv_anim_ready_cb_t complete_cb;
  // 用户数据
  void* user_data;
} xanime_param_t;
```

#### **其中必须项为：**

```c
(xanime_param_t){
    .dur = "5000",
}
```

#### **其中可以配置数值和百分比的项为：**

```c
(xanime_param_t){
    .x = "50",
    .y = "50",
    .width = "50",
    .height = "50",
    .pivot_x = "50",
    .pivot_y = "50",
    .dur = "5000",
}

#or

(xanime_param_t){
    .x = "50%",
    .y = "50%",
    .width = "50%",
    .height = "50%",
    .pivot_x = "50%",
    .pivot_y = "50%",
    .dur = "5000",
}
```

#### 有个特殊功能是反向播放动画

如果使用过 GSAP 可能比较熟悉 from 功能，是指从设定值反向执行动画到当前值，可以用于开屏加载动画

```c
(xanime_param_t){
    // 设定为反向播放
    .is_from = true,
    ...
    ...
    .dur = "5000",
}
```



## 示例

#### 创建 btn 同时往右上运动，同时变宽，变长，按照中心旋转，并且不断循环动画

```c
lv_obj_t *btn_test = lv_btn_create(lv_scr_act());
lv_obj_align(btn_test, LV_ALIGN_LEFT_MID, 0, 0);

xanime_create_single(btn_test, (xanime_param_t){
    .x = "150",
    .y = "0",
    .pivot_x = "50%",
    .pivot_y = "50%",
    .width = "20%",
    .height = "20%",
    .rotate = "3600",
    .dur = "3000",
    .loop = "-1",
});
```





