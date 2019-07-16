<template>
	<view>
		<scroll-view scroll-y="true" class="swipre-box-view">
			<checkbox-group name="personnel" class="checkbox-group" @change="checkbox">
				<view v-for="(_parentitem,_parentindex) in z_organization" :key="_parentindex">
					<view class="addressList-name-box">
						<text>{{_parentitem.listName}}</text>
						<image src="../../static/img/down_icon.png" @click="listTap" class="icon-down" :class="_parentitem.show?'icon-down-rotate':''"
						 :data-index="_parentindex" />
					</view>
					<view :hidden="!_parentitem.show">
						<view v-for="(item,sonindex) in _parentitem.item" :key="sonindex">
							<view class="addressList-item-name-box">
								<checkbox  :value="item.user.itemId" :data-index="sonindex"  class="form-checkbox"></checkbox>
								<text class="addressList-item-name"> {{item.user.itemName}}</text>
							</view>

						</view>
					</view>
				</view>
			</checkbox-group>
			<!-- //<view class="swiper-item uni-bg-red">A</view> -->
		</scroll-view>
	</view>
</template>

<script>
	var _self;
	export default {
		data() {
			return {
				z_organization: [],
				z_Id: '',
				index: 0,
				userId: ''
			};
		},
		props: {
			organization: {
				type: [Array,Object],
				default: function() {
					return [{
						listName: '部门1',
						show: false,
						item: [{
							user: {
								itemName: '人员1',
								itemId: 1
							}
						}, {
							user: {
								itemName: '人员2',
								itemId: 2
							}
						}]

					}, {
						listName: '部门2',
						show: false,
						item: [{
							user: {
								itemName: '人员3',
								itemId: 3
							}
						}, {
							user: {
								itemName: '人员4',
								itemId: 4
							}
						}]

					}]
				}

			}
		},
		async created() {
			
			_self=this;
			await _self._create();
			
		},
		watch: {
			list: function(_organization) {
				this.z_organization = this.organization;
			}
		},
		methods: {
			async _create(){
				let p=new Promise(function(resolve,reject){
					setTimeout(function(){
						if(_self.organization){
							_self.z_organization = _self.organization;
						}else{
							_self._create();
						}
					},500)
				})
			},
			async listTap(e) {
				let Index = e.currentTarget.dataset.index; //获取点击的下标值 
				const organization1 = _self.z_organization;
				organization1[Index].show = !organization1[Index].show || false; //变换其打开、关闭的状态 

				if (organization1[Index].show) { //如果点击后是展开状态，则让其他已经展开的列表变为收起状态 
					_self.$packUp(organization1, Index);
				}
				_self.z_organization = organization1;
				_self.index = Index;

			},
			checkbox(e){
				_self.userId=e.detail.value;
				_self.emit(_self.userId);
			},
			emit(userId) {
				_self.$emit('change', {
					userId: userId
				});
			}
		}

	}
</script>

<style>
@import url("../../common/plane.css");
</style>
