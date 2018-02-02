<template>
	<div>
		<Nav navtitle='产品业务'></Nav>
		<div class='all'>
		<div class='margintop' v-for='(item,index) in this.productlist' :key=index>
			<router-link :to="{name:'productcid',params:{id:{id:item.id,name:item.title}}}" class="fontstyle">
				<div class="product-box">
					<div class="product-boxl"><img :src="item.imgurl1"></div>
					<div class="product-boxr">
						<img :src="item.imgurl2">
						<p class="product-boxp1">{{item.title}}</p>
						<p class="product-boxp2">{{item.content}}
						</p>
					</div>
				</div>
			</router-link>
		</div>
		<div style="clear: both;"></div>
		</div>
	</div>
</template>

<script>
import Nav from '../common/nav.vue'
import Foot from '../common/footer.vue'
import p11 from '../../../images/products1-1.jpg'
import p21 from '../../../images/products2-1.jpg'
import p12 from '../../../images/products1-2.jpg'
import p22 from '../../../images/products2-2.jpg'
import p13 from '../../../images/products1-3.jpg'
import p23 from '../../../images/products2-3.jpg'
import p14 from '../../../images/products1-4.jpg'
import p24 from '../../../images/products2-4.jpg'
import p15 from '../../../images/products1-5.jpg'
import p25 from '../../../images/products2-5.jpg'
import p16 from '../../../images/products1-6.jpg'
import p26 from '../../../images/products2-6.jpg'
import p17 from '../../../images/products1-7.jpg'
import p27 from '../../../images/products2-7.jpg'
import p18 from '../../../images/products1-8.jpg'
import p28 from '../../../images/products2-8.jpg'
export default {
  data () {
    return {
      productlist: [
        {
          imgurl1: p11,
          imgurl2: p21,
          title: '酒水饮料',
          content: '意大利文化我们应该学习葡萄酒，意大利葡萄酒，葡萄酒和许多其他称，是家庭的 基本饮料。',
          id: '5a72ca5fd03d2b14449ff1a6'
        },
        {
          imgurl1: p12,
          imgurl2: p22,
          title: '休闲零食',
          content: '意大利有什么好吃的小吃？让我告诉你一些美味到不能停止的零食。',
          id: '5a72ca6cd03d2b14449ff1a7'
        },
        {
          imgurl1: p13,
          imgurl2: p23,
          title: '粮油副食',
          content: '意大利是一个美食国家，他们有着悠久的饮食历史，作为他们的艺术、时尚和家具，总是喜欢精心制作。',
          id: '5a72cac6d03d2b14449ff1aa'
        },
        {
          imgurl1: p14,
          imgurl2: p24,
          title: '新鲜水果',
          content: '意大利是一个美食国家，他们有着悠久的饮食历史，作为他们的艺术、时尚和家具，总是喜欢精心制作。',
          id: '5a72ca7ed03d2b14449ff1a9'
        },
        {
          imgurl1: p15,
          imgurl2: p25,
          title: '酒水饮料',
          content: '意大利文化我们应该学习葡萄酒，意大利葡萄酒，葡萄酒和许多其他称，是家庭的 基本饮料。',
          id: '5a72ca5fd03d2b14449ff1a6'
        },
        {
          imgurl1: p16,
          imgurl2: p26,
          title: '休闲零食',
          content: '意大利有什么好吃的小吃？让我告诉你一些美味到不能停止的零食。',
          id: '5a72ca6cd03d2b14449ff1a7'
        },
        {
          imgurl1: p17,
          imgurl2: p27,
          title: '粮油副食',
          content: '意大利是一个美食国家，他们有着悠久的饮食历史，作为他们的艺术、时尚和家具，总是喜欢精心制作。',
          id: '5a72cac6d03d2b14449ff1aa'
        },
        {
          imgurl1: p18,
          imgurl2: p28,
          title: '新鲜水果',
          content: '意大利是一个美食国家，他们有着悠久的饮食历史，作为他们的艺术、时尚和家具，总是喜欢精心制作。',
          id: '5a72ca5fd03d2b14449ff1a6'
        }
      ]
    }
  },
  components: {
    Nav,
    Foot
  }
}
</script>

<style scoped>
.product-box {
  width: 700px;
  display: flex;
  z-index: 11;
  position: relative;
}

.product-boxl {
  width: 348px;
}

.product-boxr {
  width: 350px;
  background: #f8efe7;
  position: relative;
  text-align: center;
}
.all {
  margin-top: 30px;
}
.product-boxr img {
  margin-top: 30px;
}

.product-boxp1 {
  color: #ee882a;
  font-weight: 600;
  font-size: 18px;
  margin: 30px 35px 0;
}

.margintop {
  margin-top: 20px;
  float: left;
}

.margintop:nth-of-type(2n) {
  margin-left: 20px;
}

.product-boxp2 {
  color: #ee882a;
  font-weight: 100;
  font-size: 14px;
  margin: 20px 35px 0;
  line-height: 25px;
  letter-spacing: 1px;
}

.product-boxl img {
  width: 100%;
}

.fontstyle {
  text-decoration: none;
  z-index: 11;
}
</style>
