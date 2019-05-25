<template>
    	<div class="citybody">
				<div class="citylist">
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
				<!-- <div class="cityindex">
					<ul>
						<li>A</li>
						<li>B</li>
						<li>C</li>
						<li>D</li>
						<li>E</li>
					</ul>
				</div> -->
			</div>
</template>

<script>
export default {
  name: "City"
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
.citybody .cityindex{ width:20px; display: flex; flex-direction:column; justify-content:center; text-align: center; border-left:1px #e6e6e6 solid;}
</style>
