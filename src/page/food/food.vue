<template>
    <div class="food_container">
    	<head-top :head-title="headTitle" goBack="true"></head-top>
    	<section class="sort_container">
			  <!-- 分类 -->
    		<div class="sort_item" :class="{choose_type:sortBy == 'food'}" >
    			<div class="sort_item_container" @click="chooseType('food')">
    				<div class="sort_item_border">
    					<span :class="{category_title: sortBy == 'food'}">{{foodTitle}}</span>
		    			<svg width="10" height="10" xmlns="http://www.w3.org/2000/svg" version="1.1" class="sort_icon">
			    			<polygon points="0,3 10,3 5,8"/>
			    		</svg>
    				</div>
    			</div>
	    		<transition name="showlist" v-show="category">
	    			<section v-show="sortBy == 'food'" class="category_container sort_detail_type">
	    				<section class="category_left">
	    					<ul>
	    						<li v-for="(item, index) in category" :key="index" class="category_left_li" :class="{category_active:restaurant_category_id == item.id}" @click="selectCategoryName(item.id, index)">
									<section>
										<img :src="getImgPath(item.image_url)" v-if="index" class="category_icon">
										<span>{{item.name}}</span>
									</section>
									<section>
	    								<span class="category_count">{{item.count}}</span>
	    								<svg v-if="index" width="8" height="8" xmlns="http://www.w3.org/2000/svg" version="1.1" class="category_arrow" >
							    			<path d="M0 0 L6 4 L0 8"  stroke="#bbb" stroke-width="1" fill="none"/>
							    		</svg>
									</section>
	    						</li>
	    					</ul>
	    				</section>
              <ul class="category_right">
                <li v-for="(item, index) in categoryDetail" :key="index" class="category_right_li" @click="getCategoryIds(item.id, item.name)" :class="{category_right_choosed: restaurant_category_ids == item.id || (!restaurant_category_ids) && index == 0}">
                  <span>{{item.name}}</span>
                  <span>{{item.count}}</span>
                </li>
              </ul>
	    			</section>
	    		</transition>
    		</div>
			  <!-- 排序 -->
    		<div class="sort_item" :class="{choose_type:sortBy == 'sort'}">
    			<div class="sort_item_container" @click="chooseType('sort')">
    				<div class="sort_item_border">
		    			<span :class="{category_title: sortBy == 'sort'}">排序</span>
		    			<svg width="10" height="10" xmlns="http://www.w3.org/2000/svg" version="1.1" class="sort_icon">
			    			<polygon points="0,3 10,3 5,8"/>
			    		</svg>
    				</div>
    			</div>
	    		<transition name="showlist">
	    			<section v-show="sortBy == 'sort'" class="sort_detail_type">
	    				<ul class="sort_list_container" @click="sortList($event)">
	    					<li class="sort_list_li">
	    						<svg>
                    <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#default"></use>
                  </svg>
	    						<p data="0" :class="{sort_select: sortByType == 0}">
	    							<span>智能排序</span>
	    							<svg v-if="sortByType == 0">
                      <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#selected"></use>
                    </svg>
	    						</p>
	    					</li>
	    					<li class="sort_list_li">
	    						<svg>
                    <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#distance"></use>
                  </svg>
                    <p data="5" :class="{sort_select: sortByType == 5}">
	    							<span>距离最近</span>
	    							<svg v-if="sortByType == 5">
                      <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#selected"></use>
                    </svg>
	    						</p>
	    					</li>
	    					<li class="sort_list_li">
	    						<svg>
                    <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#hot"></use>
                  </svg>
	    						<p data="6" :class="{sort_select: sortByType == 6}">
	    							<span>销量最高</span>
	    							<svg v-if="sortByType == 6">
                      <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#selected"></use>
                    </svg>
	    						</p>
	    					</li>
	    					<li class="sort_list_li">
	    						<svg>
                    <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#price"></use>
                  </svg>
	    						<p data="1" :class="{sort_select: sortByType == 1}">
	    							<span>起送价最低</span>
	    							<svg v-if="sortByType == 1">
                      <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#selected"></use>
                    </svg>
								  </p>
	    					</li>
	    					<li class="sort_list_li">
	    						<svg>
                    <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#speed"></use>
                  </svg>
	    						<p data="2" :class="{sort_select: sortByType == 2}">
	    							<span>配送速度最快</span>
	    							<svg v-if="sortByType == 2">
                      <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#selected"></use>
                    </svg>
	    						</p>
	    					</li>
	    					<li class="sort_list_li">
	    						<svg>
                    <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#rating"></use>
                  </svg>
	    						<p data="3" :class="{sort_select: sortByType == 3}">
	    							<span>评分最高</span>
	    							<svg v-if="sortByType == 3">
                      <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#selected"></use>
                    </svg>
	    						</p>
	    					</li>
	    				</ul>
	    			</section>
	    		</transition>
    		</div>
			  <!-- 筛选 -->
    		<div class="sort_item" :class="{choose_type:sortBy == 'activity'}">
    			<div class="sort_item_container" @click="chooseType('activity')">
	    			<span :class="{category_title: sortBy == 'activity'}">筛选</span>
	    			<svg width="10" height="10" xmlns="http://www.w3.org/2000/svg" version="1.1" class="sort_icon">
		    			<polygon points="0,3 10,3 5,8"/>
		    		</svg>
    			</div>
	    		<transition name="showlist">
	    			<section v-show="sortBy == 'activity'" class="sort_detail_type filter_container">
	    				<section style="width: 100%;">
	    					<header class="filter_header_style">配送方式</header>
	    					<ul class="filter_ul">
	    						<li v-for="(item, index) in Delivery" :key="index" class="filter_li" @click="selectDeliveryMode(item.id)">
	    							<svg :style="{opacity: (item.id == 0)&&(delivery_mode !== 0)? 0: 1}">
										<use xmlns:xlink="http://www.w3.org/1999/xlink" :xlink:href="delivery_mode == item.id? '#selected':'#fengniao'"></use>
									</svg>
	    							<span :class="{selected_filter: delivery_mode == item.id}">{{item.text}}</span>
	    						</li>
	    					</ul>
	    				</section>
	    				<section style="width: 100%;">
	    					<header class="filter_header_style">商家属性（可以多选）</header>
	    					<ul class="filter_ul" style="paddingBottom: .5rem;">
	    						<li v-for="(item,index) in Activity" :key="index" class="filter_li" @click="selectSupportIds(index, item.id)">
	    							<svg v-show="support_ids[index].status" class="activity_svg">
										<use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#selected"></use>
									</svg>
	    							<span class="filter_icon" :style="{color: '#' + item.icon_color, borderColor: '#' + item.icon_color}" v-show="!support_ids[index].status">{{item.icon_name}}</span>
	    							<span :class="{selected_filter: support_ids[index].status}">{{item.name}}</span>
	    						</li>
	    					</ul>
	    				</section>
	    				<footer class="confirm_filter">
	    					<div class="clear_all filter_button_style" @click="clearSelect">清空</div>
	    					<div class="confirm_select filter_button_style" @click="confirmSelectFun">确定<span v-show="filterNum">({{filterNum}})</span></div>
	    				</footer>
	    			</section>
	    		</transition>
    		</div>
    	</section>
    	<transition name="showcover">
    		<div class="back_cover" v-show="sortBy"></div>
    	</transition>
    	<section class="shop_list_container">
	    	<shop-list :geohash="geohash" :restaurantCategoryId="restaurant_category_id" :restaurantCategoryIds="restaurant_category_ids" :sortByType='sortByType' :deliveryMode="delivery_mode" :confirmSelect="confirmStatus" :supportIds="support_ids" v-if="latitude"></shop-list>
    	</section>
    </div>
</template>

<script>
import { mapState, mapMutations } from "vuex";
import headTop from "src/components/header/head";
import shopList from "src/components/common/shoplist";
import { getImgPath } from "src/components/common/mixin";
import {
  msiteAddress,
  foodCategory,
  foodDelivery,
  foodActivity
} from "src/service/getData";

export default {
  components: {
    headTop,
    shopList
  },
  data() {
    return {
      geohash: "", // city页面传递过来的地址geohash
      headTitle: "", // msiet页面头部标题
      foodTitle: "", // 排序左侧头部标题
      restaurant_category_id: "", // 食品类型id值
      restaurant_category_ids: "", //筛选类型的id
      sortBy: "", // 筛选的条件
      category: null, // category分类左侧数据
      categoryDetail: null, // category分类右侧的详细数据
      sortByType: null, // 根据何种方式排序
      Delivery: null, // 配送方式数据
      Activity: null, // 商家支持活动数据
      delivery_mode: null, // 选中的配送方式
      support_ids: [], // 选中的商铺活动列表
      filterNum: 0, // 所选中的所有样式的集合
      confirmStatus: false // 确认选择
    };
  },
  created() {
    this.initData();
  },
  mixins: [getImgPath],
  computed: {
    ...mapState(["latitude", "longitude"])
  },
  methods: {
    ...mapMutations(["RECORD_ADDRESS"]),
    //初始化获取数据
    async initData() {
      //获取从msite页面传递过来的参数
      this.geohash = this.$route.query.geohash;
      this.headTitle = this.$route.query.title;
      this.foodTitle = this.headTitle;
      this.restaurant_category_id = this.$route.query.restaurant_category_id;
      //防止刷新页面时，vuex状态丢失，经度纬度需要重新获取，并存入vuex
      if (!this.latitude) {
        //获取位置信息
        let res = await msiteAddress(this.geohash);
        // 记录当前经度纬度进入vuex
        this.RECORD_ADDRESS(res);
      }
      //获取category分类左侧数据
      this.category = await foodCategory(this.latitude, this.longitude);
      //初始化时定位当前category分类左侧默认选择项，在右侧展示出其sub_categories列表
      this.category.forEach(item => {
        if (this.restaurant_category_id == item.id) {
          this.categoryDetail = item.sub_categories;
        }
      });
      //获取筛选列表的配送方式
      this.Delivery = await foodDelivery(this.latitude, this.longitude);
      //获取筛选列表的商铺活动
      this.Activity = await foodActivity(this.latitude, this.longitude);
      //记录support_ids的状态，默认不选中，点击状态取反，status为true时为选中状态
      this.Activity.forEach((item, index) => {
        this.support_ids[index] = { status: false, id: item.id };
      });
    },
    // 点击顶部三个选项，展示不同的列表，选中当前选项进行展示，同时收回其他选项
    async chooseType(type) {
      if (this.sortBy !== type) {
        this.sortBy = type;
        //food选项中头部标题发生改变，需要特殊处理
        if (type == "food") {
          this.foodTitle = "分类";
        } else {
          //将foodTitle 和 headTitle 进行同步
          this.foodTitle = this.headTitle;
        }
      } else {
        //再次点击相同选项时收回列表
        this.sortBy = "";
        if (type == "food") {
          //将foodTitle 和 headTitle 进行同步
          this.foodTitle = this.headTitle;
        }
      }
    },
    //选中Category左侧列表的某个选项时，右侧渲染相应的sub_categories列表
    selectCategoryName(id, index) {
      //第一个选项 -- 全部商家 因为没有自己的列表，所以点击则默认获取选所有数据
      if (index === 0) {
        this.restaurant_category_ids = null;
        this.sortBy = "";
        //不是第一个选项时，右侧展示其子级sub_categories的列表
      } else {
        this.restaurant_category_id = id;
        this.categoryDetail = this.category[index].sub_categories;
      }
    },
    //选中Category右侧列表的某个选项时，进行筛选，重新获取数据并渲染
    getCategoryIds(id, name) {
	  console.log(id, name)
      this.restaurant_category_ids = id;
      this.sortBy = "";
      this.foodTitle = this.headTitle = name;
    },
    //点击某个排序方式，获取事件对象的data值，并根据获取的值重新获取数据渲染
    sortList(event) {
      let node;
      // 如果点击的是 span 中的文字，则需要获取到 span 的父标签 p
      if (event.target.nodeName.toUpperCase() !== "P") {
        node = event.target.parentNode;
      } else {
        node = event.target;
      }
      this.sortByType = node.getAttribute("data");
      this.sortBy = "";
    },
    //筛选选项中的配送方式选择
    selectDeliveryMode(id) {
      //delivery_mode为空时，选中当前项，并且filterNum加一
      if (this.delivery_mode == null) {
        this.filterNum++;
        this.delivery_mode = id;
        //delivery_mode为当前已有值时，清空所选项，并且filterNum减一
      } else if (this.delivery_mode == id) {
        this.filterNum--;
        this.delivery_mode = null;
        //delivery_mode已有值且不等于当前选择值，则赋值delivery_mode为当前所选id
      } else {
        this.delivery_mode = id;
      }
    },
    //点击商家活动，状态取反
    selectSupportIds(index, id) {
      //数组替换新的值
      this.support_ids.splice(index, 1, {
        status: !this.support_ids[index].status,
        id
      });
      //重新计算filterNum的个数
      this.filterNum = this.delivery_mode == null ? 0 : 1;
      this.support_ids.forEach(item => {
        if (item.status) {
          this.filterNum++;
        }
      });
    },
    //只有点击清空按钮才清空数据，否则一直保持原有状态
    clearSelect() {
      this.support_ids.map(item => (item.status = false));
      this.filterNum = 0;
      this.delivery_mode = null;
    },
    //点击确认时，将需要筛选的id值传递给子组件，并且收回列表
    confirmSelectFun() {
      //状态改变时，因为子组件进行了监听，会重新获取数据进行筛选
      this.confirmStatus = !this.confirmStatus;
      this.sortBy = "";
    }
  }
};
</script>

<style lang="scss" scoped>
@import "src/style/food";
</style>
