                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 <template>
	<div class="header">
    <!-- .header 的子元素 1 -->
		<div class="content-wrapper">
      <!-- .content-wrapper 的子元素 1 -->
			<div class="head_img">
				<img :src="seller.avatar" alt="" style="width: 64px; height: 64px;" />
			</div>
      <!-- .content-wrapper 的子元素 2 -->
			<div class="content">
        <!-- .content 的子元素 1 -->
				<div class="title">
					<span class="brand"></span>
					<span class="name">{{seller.name}}</span>
				</div>
        <!-- .content 的子元素 2 -->
				<div class="desc">
					{{ seller.description }}/{{ seller.deliveryTime }}分钟送达
				</div>
        <!-- .content 的子元素 3 -->
        <!-- 避免父组件传来的 seller 是空对象 | 接收成功为 true 时渲染其中的内容 -->
				<div v-if="seller.supports" class="supports">
					<span class="icon" :class="classMap[seller.supports[1].type]"></span>
					<span class="text">{{ seller.supports[1].description }}</span>
				</div>
			</div>
		</div>
    <!-- .header 的子元素 2 -->
    <div v-if="seller.supports" class="supportsbtn" @click="show_Detail">
      <span class="count">{{ seller.supports.length }}个</span>
      <i class="icon icon-keyboard_arrow_right"></i>
    </div>
    <!-- .header 的子元素 3 -->
		<div class="bulletin-wrapper" @click="show_Detail">
			<span class="title"></span>
			<div class=" text out_text_hidden">{{ seller.bulletin }}</div>
			<i class="icon icon-keyboard_arrow_right"></i>
		</div>
		<!-- .header 的子元素 4 -->
		<!-- 头部背景图 -->
    <div class="bg_img">
      <img :src="seller.avatar" alt="" style="width: 100%; height: 100%;" />
    </div>
		<!-- .header 的子元素 5 -->
		<!-- 过渡效果 -->
		<transition name="fade">
			<div v-show="detail_Show" class="detail">
				<!-- .detail 的子元素 1 -->
				<div class="content">
					<h1 class="name">{{ seller.name }}</h1>
					<!-- 向子组件传参 | 星星 -->
					<star :size="20" :score="seller.score"></star>
					<div class="title">
						<div class="line"></div>
						<div class="text">优惠信息</div>
						<div class="line"></div>
					</div>
					<ul v-if="seller.supports" class="supports">
						<li class="support-item" v-for="item in seller.supports">
							<icon-type :size="16" :index="item.type"></icon-type>
							<span class="text">{{ item.description }}</span>
						</li>
					</ul>
					<div class="title">
						<div class="line"></div>
						<div class="text">商家公告</div>
						<div class="line"></div>
					</div>

					<div class="bulletin">
						<p>{{ seller.bulletin }}</p>
					</div>
				</div>
				<!-- .detail 的子元素 2 -->
				<!-- 关闭弹窗 -->
				<div class="close">
					<span class="icon icon-close" @click="close_Detail"></span>
				</div>
			</div>
		</transition>
	</div>
</template>

<script>
// 导入子组件
import star from '../public/star/star.vue';
import iconType from '../public/iconType/iconType.vue';

export default {
	data() {
		return {
			// 初始化
			classMap: [],
			detail_Show: false
		};
	},
  props: {
    // 用于接收父组件的传来的数据 |
    seller: {
      // 验证类型： Object
      type: Object
    }
  },
  // 调用 vue 生命周期函数 (实例创建成功后调用)
  created: function() {
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
  },
	methods: {
		show_Detail() {
			this.detail_Show = true;
		},
		close_Detail() {
			this.detail_Show = false;
		}
	},
	// 注册组件
	components: {
		star,
		iconType
	}
};
</script>

<!-- scoped 属性，表示样式仅对当前组件以及其子组件的模板生效 -->
<style type="text/css" lang="less" scoped>
	.header {
    position: relative;   /* 为子元素定位做铺垫 */
		background: rgba(7, 17, 27, .5);
		color: #fff;
      /*.header 的子元素 1 */
    .content-wrapper {
      padding: 24px 0 18px 24px;
      display: flex;
      /* .content-wrapper 的子元素 1 */
      .head_img {
        display: inline-block;   /* 设置元素为行内块级元素 | 可设置宽高 */
        width: 64px;
        height: 64px;
        margin-right: 16px;
        img {
          border-radius: 50%;   /* 设置元素圆角效果 */
					border: 2px solid #009688;
        }
      }
      /* .content-wrapper 的子元素 2 */
      .content {
        display: flex;   /* 弹性布局 */
        flex-direction: column;   /* 主轴为垂直方向，起点在上沿 */
        justify-content: space-between;   /* 项目在主轴上的对齐方式：两端对齐，项目之间的间隔都相等 */
        height: 64px;
        /* .content 的子元素 1 */
        .title {
          height: 18px;
          line-height: 18px;
          font-size: 0;
          font-weight: bold;
          .brand {
            display: inline-block;
            width: 30px;
            height: 18px;
            margin-right: 6px;
            background: url('brand@2x.png') no-repeat;   /* 设置背景图片 且 不重复 */
            background-size: 100% 100%;   /* 指定背景图片大小：相对于背景定位区域的百分比 | 前者：宽度，后者：高度 */
          }
          .name {
            vertical-align: top;   /* 设置一个元素的垂直对齐方式：把元素的顶端与行中最高元素的顶端对齐 */
            font-size: 16px;
          }
        }
        /* .content-wrapper 的子元素 2 */
        .desc {
          height: 12px;
          font-size: 12px;
          font-weight: 200;   /* 设置文本的粗细 */
          line-height: 12px;
        }
        /* .content-wrapper 的子元素 3 */
        .supports {
          height: 10px;
          font-size: 10px;
          font-weight: 200;
          line-height: 10px;
          font-size: 0;
          .icon {
            display: inline-block;
            width: 12px;
            height: 12px;
            background-size: 100% 100%;
            background-repeat: no-repeat;
            vertical-align: top;
             /*使用 & 引用父选择器 */
            &.decrease {
              background-image: url('decrease_2@2x.png');
            }
            &.discount {
              background-image: url('discount_2@2x.png');
            }
            &.invoice {
              background-image: url('invoice_2@2x.png');
            }
            &.special {
              background-image: url('special_2@2x.png');
            }
            &.guarantee {
              background-image: url('guarantee_2@2x.png');
            }
          }
          .text {
            font-size: 10px;
            margin-left: 4px;
          }
        }
      }
    }
    /* .header 的子元素 2 */
		.supportsbtn {
			position: absolute;   /* 设置元素：绝对定位 */
			right: 12px;
			bottom: 40px;
			background: rgba(0, 0, 0, .2);
			height: 24px;
			line-height: 24px;
			font-size: 0;
			border-radius: 12px;
			padding: 0 8px;
			.count {
				font-size: 10px;
				margin-right: 2px;
			}
			.icon {
				font-size: 12px;
				line-height: 24px;
				vertical-align: top;
			}
		}
    /* .header 的子元素 3 */
    .bulletin-wrapper {
      height: 28px;
      position: relative;
      background: rgba(7, 17, 27, .2);
      .title {
        position: absolute;
        top: 8px;
        left: 12px;
        display: inline-block;
        width: 22px;
        height: 12px;
        background-image: url('bulletin@2x.png');
        background-size: 100% 100%;
        background-repeat: no-repeat;
      }
      .text {
        display: block;
        padding: 0 30px 0 38px;
        line-height: 28px;
        font-size: 10px;
      }
      .out_text_hidden {
        white-space: nowrap;   /* 指定元素内的空白怎样处理: 不换行 */
        overflow: hidden;   /* 内容溢出一个元素的框: 隐藏 */
        text-overflow: ellipsis;   /* 指定当文本溢出包含它的元素:显示省略符号 */
      }
      .icon {
        position: absolute;
        top: 0;
        right: 12px;
        line-height: 28px;
        font-size: 16px;
      }
    }
    /* .header 的子元素 4 */
    .bg_img {
      position: absolute;
      top: 0;
      left: 0;
      /*z-index 属性仅在节点的 position 属性为 relative, absolute 或者 fixed 时生效.*/
      z-index: -1;   /* 指定一个元素的堆叠顺序 */
			width: 100%;
			height: 100%;
			filter: blur(5px);   /* 定义了元素(通常是<img>)的可视效果 | 高斯模糊 */
			-webkit-filter: blur(5px);   /* 兼容 chrome & safari */

    }
		/* .header 的子元素 5 */
		.detail {
			position: fixed;   /* 相对浏览器固定定位 */
			top: 0;
			left: 0;
			z-index: 10;   /* 显示于其它元素上面 */
			width: 100%;
			height: 100%;
			display: flex;   /* 弹性布局 */
			flex-direction: column;   /* 定义主轴方向 |垂直方向，起点在上沿 */
			background: rgba(7, 17, 27, .8);
			overflow: auto;   /* 指定内容溢出一个元素的框时：显示滚动条 */
			transition: all .5s;   /* 过渡元素为 opacity, 持续 1s*/
			&.fade-enter-active, &.fade-leave {   /* 定义进入过渡的结束状态 & 离开过渡的开始状态 */
				opacity: 1;
				background: rgba(7, 17, 27, .8);
			}
			&.fade-enter, &.fade-leave-active {   /* 定义进入过渡的开始状态 & 离开过渡的结束状态 */
				opacity: 0;
				background: rgba(7, 17, 27, 0);
			}
			/*. detail 的子元素 1 */
			.content {
				flex: 1;   /* 定义项目等比例放大 */
				text-align: center;
				.name {
					font-size: 16px;
					font-weight: 700;   /* 字体粗细 */
					color: #009688;
					text-align: center;
					margin: 64px 0 16px 0;
				}
				.title {
					display: flex;
					width: 80%;
					margin: 28px auto 24px auto;
					.line {
						flex: 1;
						margin-bottom: 6px;   /* 下边距 */
						border-bottom: 1px solid rgba(255, 255, 255, .2);  /* 设置底部边框样式 */
					}
					.text {
						padding: 0 12px;   /* 元素内边距 */
						font-size: 14px;
						color: #009688;
						font-weight: 600;
					}
				}
				.supports {
					text-align: left;
					font-size: 12px;
					color: #009688;
					font-weight: bold;
					width: 80%;
					margin: 0 auto;   /* 水平居中 */
					padding: 0 12px;
					.support-item {
						margin-bottom: 12px;
						padding: 0 12px;
						.text {
							font-size: 12px;
							margin-left: 6px;
							line-height: 16px;
						}
						/* 父级元素的最后个子元素 */
						&:last-child {
							margin-bottom: 0;
						}
					}
				}
				.bulletin {
					text-align: left;   /* 文本排列在左边 */
					width: 80%;
					margin: 0 auto;
					> p {
						font-size: 12px;
						color: #009688;
						line-height: 2;   /* 行高 | 设置数字，此数字会与当前的字体尺寸相乘来 设置行间距 */
						padding: 0 12px;
					}
				}
			}
			/* .detail 的子元素 2 */
			.close {
				text-align: center;
				margin: 32px 0;   /* 外边距 */
				.icon {
					font-size: 32px;
					color: #009688;
				}
			}
		}
	}
</style>
