<template>
    	<!-- <div class="citybody"> -->
				<!-- <div class="citylist">
					<div class="cityhot">
						<h2>热门城市</h2>
						<ul class="clearfix">
							<li>上海</li>
							<li>北京</li>
							<li>上海</li>
							<li>北京</li>
							<li>上海</li>
							<li>北京</li>
							<li>上海</li>
							<li>北京</li>
						</ul>
					</div>
					<div class="citysort">
						<div>
							<h2>A</h2>
							<ul>
								<li>阿拉善盟</li>
								<li>鞍山</li>
								<li>安庆</li>
								<li>安阳</li>
							</ul>
						</div>
						<div>
							<h2>B</h2>
							<ul>
								<li>北京</li>
								<li>保定</li>
								<li>蚌埠</li>
								<li>包头</li>
							</ul>
						</div>
						<div>
							<h2>A</h2>
							<ul>
								<li>阿拉善盟</li>
								<li>鞍山</li>
								<li>安庆</li>
								<li>安阳</li>
							</ul>
						</div>
						<div>
							<h2>B</h2>
							<ul>
								<li>北京</li>
								<li>保定</li>
								<li>蚌埠</li>
								<li>包头</li>
							</ul>
						</div>
						<div>
							<h2>A</h2>
							<ul>
								<li>阿拉善盟</li>
								<li>鞍山</li>
								<li>安庆</li>
								<li>安阳</li>
							</ul>
						</div>
						<div>
							<h2>B</h2>
							<ul>
								<li>北京</li>
								<li>保定</li>
								<li>蚌埠</li>
								<li>包头</li>
							</ul>
						</div>	
					</div>
				</div>
				<div class="cityindex">
					<ul>
						<li>A</li>
						<li>B</li>
						<li>C</li>
						<li>D</li>
						<li>E</li>
					</ul>
				</div> -->

				<!-- 更改数据 -->
				<!-- <div class="citylist">
					<div class="cityindex">
					<ul>
						<li v-for="(item,index) in cityList" :key = "item.index" @touchstart="handleToIndex(index)">{{ item.index }}</li>
					</ul>
				</div>
					<div class="cityhot">
						<h2>热门城市</h2>
						<ul class="clearfix">
							<li v-for="item in hotList" :key="item.id">{{ item.nm }}</li>
						</ul>
					</div>
					<div class="citysort" ref="city_sort">
						<div v-for="item in cityList" :key="item.index">
							<h2>{{ item.index }}</h2>
							<ul>
								<li v-for="itemList in item.list" :key="itemList.id">{{ itemList.nm }}</li>
							</ul>
						</div>
					</div>
					
				</div>
			</div> -->


			<div class="citybody">
				<div class="citylist" ref="city_list" id="scroll">
					<div class="cityindex">
						<ul>
							<li v-for="(item,index) in cityList" :key = "item.index" @touchstart="handleToIndex(index)">{{ item.index }}</li>
						</ul>
					</div>
			<Loading v-if="isLoading" />
					<Scroller v-else ref="city">
					<div>
						<div class="cityhot">
							<h2>热门城市</h2>
							<ul class="clearfix">
								<li v-for="item in hotList" :key="item.id" @tap="handleToCity(item.nm,item.id)">{{ item.nm }} {{ item.id }}</li> 
								<!-- 点击选择城市方法 -->
							</ul>
						</div>

						<div class="citysort" ref="city_sort">
							<div v-for="item in cityList" :key="item.index">
								<h2 class="h2">{{ item.index }}</h2>
								<ul>
									<li v-for="itemList in item.list" :key="itemList.id" @tap="handleToCity(itemList.nm,itemList.id)">{{ itemList.nm }}</li>
								</ul>
							</div>
						</div>
					</div>
					</Scroller>
				</div>	
			</div>
</template>

<script>

export default {
	name: "City",
	data(){
		return{
			cityList: [],
			hotList: [],
			isLoading : true
		}
	},
	mounted(){

		let cityList = window.localStorage.getItem('cityList');
		let hotList = window.localStorage.getItem('hotList');

		if(cityList && hotList){ //说明本地存储已经存储了  就走本地存储
			this.cityList = JSON.parse(cityList)
			this.hotList = JSON.parse(hotList)
			this.isLoading = false;
		}
		else{
		this.axios.get('/api/cityList').then((res)=>{
		//原生js实现一下城市分组
			const msg = res.data.msg;
			if(msg === 'ok'){
				this.isLoading = false;
				let cities = res.data.data.cities;
				//把数据改造成这样有利于渲染
				// [ { index : 'A' , list : [{ nm : '阿城' , id : 123 }] } ] 
				const { cityList , hotList } = this.formatCityList(cities);
				this.cityList = cityList;
				this.hotList = hotList;
				//开始做本地存储
				window.localStorage.setItem('cityList', JSON.stringify(cityList));
				window.localStorage.setItem('hotList', JSON.stringify(hotList));
			}
		})
		}
	},
	methods : {
		formatCityList(cities){
				const cityList = [];
				const hotList = [];

				for(let i = 0;i < cities.length;i++){
					if(cities[i].isHot === 1){
						hotList.push( cities[i] )
					}
				}

				for(let i = 0;i<cities.length;i++){
						let firstLetter = cities[i].py.substring(0,1).toUpperCase();
						//如果firstLetter在结果集中就走false 不在就走true
						if(toCom(firstLetter)){//新添加index
							cityList.push({ index : firstLetter , list : [ { nm : cities[i].nm , id : cities[i].id } ] })
						}else{ //累加到已有index中  
							for(var j=0;j<cityList.length;j++){
								if(cityList[j].index === firstLetter ){
								   cityList[j].list.push( { nm : cities[i].nm , id : cities[i].id } )
								}
							} 
						}
				}

				cityList.sort(( n1,n2 ) => {  // 排序
					if( n1.index > n2.index){
						return 1;
					}
					else if(n1.index < n2.index){
						return -1;
					}
					else{
						return 0;
					}
				})
				//确定数组里有没有索引   有返回 false 没有返回 true
				function toCom(firstLetter){
					for(var i = 0;i<cityList.length;i++){
						if(cityList[i].index === firstLetter){
							return false;
						}
					}
					return true;
				}
				return {
					cityList,
					hotList
				}
		},
		handleToIndex(index){
			 var h2 = this.$refs.city_sort.getElementsByTagName('h2');
			//   this.$nextTick(() => {
//              document.documentElement.scrollTop =  h2[index].offsetTop + 46;
//				加上better-scroll原生跳转不好使了
//                 })

			//this.$refs.city_sort.parentNode.scrollTop = h2[index].offsetTop;
			//不好使 

			this.$refs.city.toScrollTop(-h2[index].offsetTop);
			
			//因为网上滚动要加负号
        },
		handleToCity(nm,id){
			console.log(nm,id)
			this.$store.commit('city/CITY_INFO',{ nm , id });
			//牛逼啊
			window.localStorage.setItem('nowNm',nm);
			window.localStorage.setItem('nowId',id);
			//避免刷新状态消失
			this.$router.push('/movie/nowPlaying')
		}
	}
};
</script>

<style scoped>
.citybody .citylist{ flex:1; overflow: auto; background: #FFF5F0;}
.citybody .citylist::-webkit-scrollbar{
    background-color:transparent;
    width:0;
}
.citybody .cityhot{ margin-top: 20px;}
.citybody .cityhot h2{ padding-left: 15px; line-height: 30px; font-size: 14px; background:#F0F0F0; font-weight: normal;}
.citybody .cityhot ul{}
.citybody .cityhot ul li{ float: left; background: #fff; width: 29%; height: 33px; margin-top: 15px; margin-left: 3%; padding: 0 4px; border: 1px solid #e6e6e6; border-radius: 3px; line-height: 33px; text-align: center; box-sizing: border-box;}
.citybody .citysort{}
.citybody .citysort div{ margin-top: 20px;}
.citybody .citysort h2{ padding-left: 15px; line-height: 30px; font-size: 14px; background:#F0F0F0; font-weight: normal;}
.citybody .citysort ul{ padding-left: 10px; margin-top: 10px;}
.citybody .citysort ul li{ line-height: 30px; line-height: 30px;}
/* .citybody .cityindex{ display: flex; flex-direction:column; justify-content:center; text-align: center; border-left:1px #FFF5F0 solid;} */
.cityindex{ position: fixed; top : 17%; left: 95%; width:20px;display: flex; flex-direction:column; justify-content:center; text-align: center;z-index: 9999;}
@media (max-width: 320px){.cityindex{ position: fixed; top : 7%; left: 95%; width:20px;display: flex; flex-direction:column; justify-content:center; text-align: center;}}
#scroll{height: 684px ;}
</style>
