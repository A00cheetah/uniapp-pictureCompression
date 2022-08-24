<template>
	<view class="visitorsBox">
		<view class="visitorsTitle">
			来返人员新增
		</view>
		<view class="visitorsTitleContent">
			
			<!-- 身份证号码 -->
			<view class="idCard">
				<view class="idCardLeft">
					<text class="startColor">*</text>
					<text class="visitorsListName">身份证号码</text>
				</view>
				<view class="idCardContent">
					<input @input="toUp" class="inputColor" v-model="idNum" type="idcard" placeholder-style="color:#c7ccd3;" placeholder="请输入身份证号码" />
				</view>
			</view>
			
			<!-- 判断有没有这个人 -->
			<view class="yesIsHide">
				<!-- 姓名 -->
				<view class="idCard">
					<view class="idCardLeft">
						<text class="startColor">*</text>
						<text class="visitorsListName">姓名</text>
					</view>
					<view class="idCardContent">
						<input @input="toName" @blur="demoBlur" class="inputColor" v-model="name" type="text" placeholder-style="color:#c7ccd3;" placeholder="请输入姓名" />
					</view>
				</view>
				
				<!-- 手机号码 -->
				<view class="idCard">
					<view class="idCardLeft">
						<text class="startColor">*</text>
						<text class="visitorsListName">手机号码</text>
					</view>
					<view class="idCardContent">
						<input @input="toNumber" class="inputColor" v-model="mobile" type="text" placeholder-style="color:#c7ccd3;" placeholder="请输入手机号码" />
					</view>
				</view>
				
				<!-- 民族 -->
				<view class="idCard">
					<view class="idCardLeft">
						<text class="startColor">*</text>
						<text class="visitorsListName">民族</text>
					</view>
					<view class="idCardContent">
						<view class="ownmsgNameRight">
							<picker :range="nationArr" @change="nationChange" class="counterNameSelect">
								<input 
									v-if="nationArrIndex == -1" 
									type="text" 
									placeholder-style="color:#c7ccd3;line-height: 84rpx;height: 70rpx;" 
									placeholder="请选择" 
									disabled="disabled"
								/>
								<view  v-else class="selectList">{{nationArr[nationArrIndex]}}</view>
							</picker>
						</view>
					</view>
				</view>
				
				<!-- 户口所在地 -->
				<view class="idCard">
					<view class="idCardLeft">
						<text class="startColor">*</text>
						<text class="visitorsListName">户口所在地</text>
					</view>
					<view class="idCardContent">
						<input 
							class="inputColor" 
							v-model="nativePlace" 
							type="text" 
							placeholder-style="color:#c7ccd3;" 
							placeholder="请输入请输入户口所在地" 
							@input="checkNum($event)" 
						/>
					</view>
				</view>
				
			</view>
			
			<!-- 现居住地址 -->
			<view class="idCard">
				<view class="idCardLeft">
					<text class="startColor">*</text>
					<text class="visitorsListName">现居住地址</text>
				</view>
				<view class="idCardContent">
					<input 
						class="inputColor" 
						v-model="residencePlace" 
						type="text" 
						placeholder-style="color:#c7ccd3;" 
						placeholder="请输入请输入现居住地址" 
						@input="checkNum($event)" 
					/>
				</view>
			</view>
			
			<!-- 接种针数 -->
			<view class="idCard">
				<view class="idCardLeft">
					<text class="startColor">*</text>
					<text class="visitorsListName">接种针数</text>
				</view>
				<view class="idCardContent">
					<input class="inputColor" v-model="inoculation" type="number" placeholder-style="color:#c7ccd3;" placeholder="请输入请输入接种针数" @input="checkNum($event)" />
				</view>
			</view>
			
			
			<!-- 健康码状态 -->
			<view class="idCard">
				<view class="idCardLeft">
					<text class="startColor">*</text>
					<text class="visitorsListName">健康码状态</text>
				</view>
				<view class="idCardContent">
					<view class="ownmsgNameRight">
						<picker :range="healthCodeList" @change="educationChange" class="counterNameSelect">
							<input v-if="healthCodeIndex == -1" type="text" placeholder-style="color:#c7ccd3;line-height: 84rpx;height: 70rpx;" placeholder="请选择" disabled="disabled"/>
							<view  v-else class="selectList">{{healthCodeList[healthCodeIndex]}}</view>
						</picker>
					</view>
				</view>
			</view>
			
			
			<!-- 行程 -->
			<view class="idCard">
				<view class="idCardLeft">
					<text class="startColor">*</text>
					<text class="visitorsListName">行程</text>
				</view>
				<view class="idCardContent">
					<input class="inputColor" v-model="trip" type="text" placeholder-style="color:#c7ccd3;" placeholder="请输入近14天行程" />
				</view>
				<view class="idCardTips">
						<image src="../../static/tips.png" mode="" @click="showIdcardImg"></image>
					<view class="idCardTipsImg" v-show="idCardImg">
						<image src="../../static/tongxingma.png" mode=""></image>
					</view>
				</view>
			</view>
			
			
			
			<!--标签
			 <view class="idCard">
				<view class="idCardLeft">
					<text class="startColor" style="color:#fff;">*</text>
					<text class="visitorsListName">标签</text>
				</view>
				<view class="idCardContent">
					<input 
					class="inputColor" 
					v-model="lable" 
					type="text" 
					placeholder-style="color:#c7ccd3;" 
					placeholder="请输入标签" />
				</view>
			</view> -->
			
			
			<!-- 来返日期	 -->
			<view class="idCard">
				<view class="idCardLeft">
					<text class="startColor">*</text>
					<text class="visitorsListName">来返日期</text>
				</view>
				<view class="idCardContent">
					<uni-calendar
					ref="calendar"
					:insert="false"
					@confirm="confirm"
					 />
					<input style="color: rgb(199, 204, 211); line-height: 84rpx; height: 70rpx;text-indent: 20rpx;font-size: 28rpx;color:grey;" type="text" placeholder-style="color:#c7ccd3;" :placeholder="goTime" @click="open" disabled="disabled"/>
				</view>
			</view>
			
			<!-- 人脸照片 -->
			<view class="idCard">
				<view class="idCardLeft">
					<text class="startColor">*</text>
					<text class="visitorsListName">人脸照片</text>
				</view>
				<form action="">
					<view class="cu-form-group">
						<view class="grid col-4 grid-square flex-sub">
							<view class="bg-img" v-for="(item, index) in photoList"  style="width: 200rpx;height: 200rpx;" :key="index" @tap="ViewImage">
								<image :src="photoList[index]" mode="aspectFill"></image>
								<view class="cu-tag bg-red" @tap.stop="DelImg" :data-index="index">
									<text class="cuIcon-close"></text>
								</view>
							</view>
							<view class="solids" style="width: 200rpx;height: 200rpx;" @tap="ChooseImage" v-if="photoList.length<1">
								<text class="cuIcon-cameraadd"></text>
								
							</view>
						</view>
					</view>
				</form>
				
				<view class="photoText">
					请上传  大小不超过 <text> 5MB </text> 格式为 <text> png/jpg/jpeg </text> 的文件
				</view>
			
			</view>
			
			<!-- 核酸检测报告 -->
			<view class="idCard">
				<view class="idCardLeft">
					<text class="startColor">*</text>
					<text class="visitorsListName">核酸检测报告</text>
				</view>
				<form action="">
					<view class="cu-form-group">
						<view class="grid col-4 grid-square flex-sub">
							<view class="bg-img" v-for="(item, index) in nucleicAcidList"  style="width: 200rpx;height: 200rpx;" :key="index" @tap="nucleicAcid_report">
								<image :src="nucleicAcidList[index]" mode="aspectFill"></image>
								<view class="cu-tag bg-red" @tap.stop="nucleicAcid_report_DelImg" :data-index="index">
									<text class="cuIcon-close"></text>
								</view>
							</view>
							<view class="solids" style="width: 200rpx;height: 200rpx;" @tap="nucleicAcid_report_Image" v-if="nucleicAcidList.length<1">
								<text class="cuIcon-cameraadd"></text>
							</view>
						</view>
					</view>
				</form>
				
				<view class="photoText">
					请上传  大小不超过 <text> 5MB </text> 格式为 <text> png/jpg/jpeg </text> 的文件
				</view>
			
			</view>
			
			<!-- 行程码截图 -->
			
			
			
			
		</view>
		
		<view class="submitButton">
			<button type="default" v-if="submitIF" @tap="visitors_submit">提交</button>
			<button style="background-color: #8799A3;" type="default" v-else @tap="submitTest">每日0点刷新</button>
		</view>
	
	
	
	</view>
</template>

<script>
	export default{
		data(){
			return{
				name: '',
				mobile: '',
				idNum: '',
				nationArr: [
						"汉族", "壮族", "满族", "回族", "苗族", 
						"维吾尔族", "土家族", "彝族", "蒙古族",
						"藏族", "布依族", "侗族", "瑶族",
						"朝鲜族", "白族", "哈尼族",
						"哈萨克族","黎族","傣族",
						"畲族","傈僳族","仡佬族",
						"东乡族","高山族","拉祜族",
						"水族","佤族","纳西族",
						"羌族","土族","仫佬族",
						"锡伯族",
						"柯尔克孜族", "达斡尔族",
						"景颇族", "毛南族", "撒拉族",
						"布朗族", "塔吉克族", "阿昌族",
						"普米族", "鄂温克族", "怒族", 
						"京族", "基诺族", "德昂族",
						"保安族",
						"俄罗斯族", "裕固族",
						"乌孜别克族", "门巴族",
						"鄂伦春族", "独龙族", 
						"塔塔尔族", "赫哲族",
						"珞巴族"
				],
				nationArrIndex: -1,
				nationValue: '',
				nativePlace: '',
				residencePlace: '',
				inoculation: '',
				healthCodeList: ['绿码','黄码','红码'],
				healthCodeIndex: -1,
				healthCode: '',
				trip: '',
				idCardImg: false,
				lable: '',
				goTime: '选择日期',
				registrationTime:'',
				photoList: [],
				photo: '',
				photo_timer:60,
				photo_title:'人脸照片',
				nucleicAcidList: [],
				nucleicAcid: '',
				timer:60,
				title:'核酸检测报告',
				submit_title: '提交',
				shengyuH: '',
				submitIF: true,
				deptId:"",
				remainTime: ''
					
			}
		},
		onLoad(option) {
			
			this.deptId = option.deptId ? option.deptId : 123;
			let that = this
			// uni.clearStorage();
			
			
			const delDemoKey = uni.getStorageSync('demo_key')
			if(delDemoKey == ''){
				console.log('123')
				that.nationArrIndex = -1;
				this.healthCodeIndex = -1
			}
			
			uni.getStorage({
				key: 'demo_key',
				success: function (res) {
					console.log(res.data);
					let keyData = res.data;
					that.name = keyData.name;
					that.mobile = keyData.mobile;
					that.idNum = keyData.idNum;
					that.nationArrIndex = keyData.nationArrIndex;
					that.nativePlace = keyData.nativePlace;
					that.residencePlace = keyData.residencePlace;
					that.inoculation = keyData.inoculation;
					that.healthCodeIndex = keyData.healthCodeIndex;
					that.trip = keyData.trip;
					console.log(res)
				},
			});
			
			const info = uni.getStorageSync('submit_time')
			if(info){
				let date = new Date().getDate();
				let infoArr = info.split(',');
				let afterDate = infoArr.pop();
				
				if (date < afterDate || date == afterDate){
					that.submit_title = infoArr[0]
					that.submitIF = eval(infoArr[1])
					console.log('缓存',infoArr[1])
				}else{
					console.log('删除缓存')
					uni.removeStorageSync('submit_time')
					uni.removeStorageSync('demo_key')
				}
			}
		
		},
		methods:{
		
			
			//姓名
			toName(e){
				this.name = e.detail.value.toUpperCase();
			},
			
			//手机号
			toNumber(e){
				this.mobile = e.detail.value.toUpperCase();
			},
			
			// 身份证
			toUp(e){
				this.idNum = e.detail.value.toUpperCase();
			},
			
			//民族
			nationChange(e){
				this.nationArrIndex = e.target.value;
				this.nationValue = e.target.value+1;
				console.log(e.target.value,this.nationValue)
			},
			
			// 健康码状态
			educationChange(e){
				console.log(e.target.value)
				this.healthCodeIndex = e.target.value;
				this.healthCode = e.target.value+1;
				console.log(this.healthCode)
			},
			
			//行程提示图片显示
			showIdcardImg(){
				this.idCardImg = !this.idCardImg;
				console.log('鼠标按下',this.idCardImg)
			},
			
			// 接种针数
			checkNum(e) {
				if(this.inoculation > 3){
					uni.showToast({
						title: '最多只能打3针',
						icon: 'error'
					});
					this.inoculation = ''
					return;
				}
			},
			
			//来返日期
			open(){
				this.$refs.calendar.open();
				
			},
			confirm(e) {
				console.log('confirm',e);
				this.demed(e.fulldate)
			},
			demed(cd){
				console.log('demed',cd)
				let dateTime = new Date();
				let dateDay = dateTime.getDate()
				let dateMonth = dateTime.getMonth()+1;
				let currentDay = cd.split('-')[2]
				let currentMonth = cd.split('-')[1]
				if(dateMonth < currentMonth || dateMonth > currentMonth){
					uni.showToast({
						title: '只能选择本月份',
						icon: 'error'
					});
					this.goTime = '选择日期'
					this.registrationTime = ''
					return;
				}else{
					this.goTime = cd
					this.registrationTime = cd
				}
				if(dateDay < currentDay){
					uni.showToast({
						title: '只能选择今天或者之前的日期',
						icon: 'error'
					});
					this.goTime = '选择日期'
					this.registrationTime = ''
					return;
				}else{
					this.goTime = cd
					this.registrationTime = cd
				}
				
			},
			
			//人脸照片上传  图片接口http://123.156.228.92:20443/common/upload
			ChooseImage: function(){
				let _this = this;
				console.log('人脸照片',this.photo_title)
				// if(this.photo_title != '人脸照片'){
				// 	console.log('54545')
				// 	uni.showToast({
				// 		title: `${this.photo_title}`,
				// 		icon: 'error'
				// 	});
				// 	return;
				// }
				
				uni.chooseImage({
					count: 1,
					sizeType:['original', 'compressed'],
					sourceType: ['album'],
					success: (res) => {
						let resSize = res.tempFiles[0].size;
						let resType = res.tempFiles[0].type.substring(0,5)
						let resForMat = res.tempFiles[0].type.substring(6,9)
						if(resType != 'image' || resForMat == 'png' && resForMat == 'jpeg' && resForMat == 'jpg'){
							uni.showToast({
								title: 'png/jpeg/jpg',
								icon: 'error'
							});
							return;
						}
						
						
						let photoPath = '';
						
						photoPath = res.tempFilePaths.toString()
						uni.getImageInfo({
						    src: photoPath,
						    success: function (image) {
						        let canvasWidth = image.width //图片原始长宽
						        let canvasHeight = image.height;
						        let base = canvasWidth/canvasHeight;
						            //设置画布最大宽度
						            if(canvasWidth>104){
						                canvasWidth = 104;
						                canvasHeight = Math.floor(canvasWidth/base);
						            }
						            let img = new Image();  
						            img.src = photoPath; // 要压缩的图片  
									console.log(img.src)
						            let canvas = document.createElement('canvas');
						            let ctx = canvas.getContext('2d');
						            canvas.width = canvasWidth;
						            canvas.height = canvasHeight;
						            // 清除画布
						            ctx.clearRect(0, 0, canvasWidth, canvasHeight);
						            //  将图片画到canvas上面   使用Canvas压缩  
						            ctx.drawImage(img, 0, 0, canvasWidth, canvasHeight);
						            // canvas.toDataURL 返回的是一串Base64编码的URL
						            // 指定格式 PNG  
						            let imgBase = canvas.toDataURL("image/png");
									let imgBlob = _this.pngBase64ToBlob(imgBase);
									let imgUrl = _this.blobToUrl(imgBlob)
									uni.getFileInfo({
										filePath:imgUrl,
										success:imgInfo=>{
											
											if(imgInfo.size > 2097152){
												// uni.showToast({
												// 	title: '上传图片不能超过5MB',
												// 	icon: 'error'
												// });
												return;
											}
											_this.photoList.push(imgUrl)
										}
									})

									uni.uploadFile({
										url: 'http://123.156.228.92:20443/common/upload',
										filePath: imgUrl,
										name: 'file',
										formData: {
											// 'user': 'test'
										},
										success: (uploadFileRes) => {
											console.log(uploadFileRes.data);
											let resTypePng = uploadFileRes.data.indexOf('.png')
											let resType = uploadFileRes.data.substring(26,resTypePng+4)
											_this.photo = resType
										}
									});
						        }
						    });
						
						
						
						
						
						
						
					}
				});
			},
			ViewImage(e) {
				uni.previewImage({
					urls: this.photoList,
					current: e.currentTarget.dataset.url
				});
			},
			//删除人脸图片
			DelImg(e){
				uni.showModal({
					title: '删除',
					content: '确定要删除这张图片？',
					cancelText: '取消',
					confirmText: '删除',
					success: res => {
						if (res.confirm) {
							this.photoList.splice(e.currentTarget.dataset.index,1)
							this.photo = ''
							// this.photo_SMS()
						}
					}
				})
			},
			
			
			// 核酸检测报告
			nucleicAcid_report_Image: function(){
				let _this = this
				// if(this.title != '核酸检测报告'){
				// 	uni.showToast({
				// 		title: `${this.title}`,
				// 		icon: 'error'
				// 	});
				// 	return;
				// }
				uni.chooseImage({
					count: 1,
					sizeType:['original', 'compressed'],
					sourceType: ['album'],
					success: (res) => {
						let resSize = res.tempFiles[0].size;
						let resType = res.tempFiles[0].type.substring(0,5)
						let resForMat = res.tempFiles[0].type.substring(6,9)
						if(resType != 'image' || resForMat == 'png' && resForMat == 'jpeg' && resForMat == 'jpg'){
							uni.showToast({
								title: 'png/jpeg/jpg',
								icon: 'error'
							});
							return;
						}
						
						
						console.log(res)
						_this.nucleicacidPath = res.tempFilePaths.toString()
						uni.getImageInfo({
						    src: _this.nucleicacidPath,
						    success: function (image) {
						        let canvasWidth = image.width //图片原始长宽
						        let canvasHeight = image.height;
						        let base = canvasWidth/canvasHeight;
						            //设置画布最大宽度
						            if(canvasWidth>104){
						                canvasWidth = 104;
						                canvasHeight = Math.floor(canvasWidth/base);
						            }
						            let img = new Image();  
						            img.src = _this.nucleicacidPath; // 要压缩的图片  
									console.log(img.src)
						            let canvas = document.createElement('canvas');
						            let ctx = canvas.getContext('2d');
						            canvas.width = canvasWidth;
						            canvas.height = canvasHeight;
						            // 清除画布
						            ctx.clearRect(0, 0, canvasWidth, canvasHeight);
						            //  将图片画到canvas上面   使用Canvas压缩  
						            ctx.drawImage(img, 0, 0, canvasWidth, canvasHeight);
						            // canvas.toDataURL 返回的是一串Base64编码的URL
						            // 指定格式 PNG  
						            let imgBase = canvas.toDataURL("image/png");
									let imgBlob = _this.pngBase64ToBlob(imgBase);
									let imgUrl = _this.blobToUrl(imgBlob)
									uni.getFileInfo({
										filePath:imgUrl,
										success:imgInfo=>{
											
											if(imgInfo.size > 2097152){
												uni.showToast({
													title: '上传图片不能超过5MB',
													icon: 'error'
												});
												return;
											}
											_this.nucleicAcidList.push(imgUrl)
										}
									})
									
									uni.uploadFile({
										url: 'http://123.156.228.92:20443/common/upload',
										filePath: imgUrl,
										name: 'file',
										formData: {
											// 'user': 'test'
										},
										success: (uploadFileRes) => {
											console.log(uploadFileRes.data);
											let resTypePng = uploadFileRes.data.indexOf('.png')
											let resType = uploadFileRes.data.substring(26,resTypePng+4)
											_this.nucleicAcid = resType
										}
									});
									
									
						        }
						    });	
						
						
						
					}
				});
				
				
			},
			nucleicAcid_report(e) {
				uni.previewImage({
					urls: this.nucleicAcidList,
					current: e.currentTarget.dataset.url
				});
			},
			//删除核酸图片
			nucleicAcid_report_DelImg(e){
				uni.showModal({
					title: '删除',
					content: '确定要删除这张图片？',
					cancelText: '取消',
					confirmText: '删除',
					success: res => {
						if (res.confirm) {
							this.nucleicAcidList.splice(e.currentTarget.dataset.index,1)
							this.nucleicAcid = ''
							console.log('13')
							// this.nucleicAcid_SMS()
						}
					}
				})
			},
			
			// 来访人员提交
			visitors_submit(){
				var that = this
				let reg = /^[1-8][1-7]\d{4}(?:19|20)\d{2}(?:0[1-9]|1[0-2])(?:0[1-9]|[12]\d|3[01])\d{3}[\dX]$/;
				
				
				
				
				// if(reg.test(this.idNum) == false){
				// 	uni.showToast({
				// 		icon: 'none',
				// 		title: '请输入正确身份证号码',
				// 		duration: 2000
				// 	});
				// 	return
				// }
				
				if(this.inoculation == '' || this.inoculation > 3){
					uni.showToast({
						icon: 'none',
						title: '最多只能打3针',
						duration: 2000
					});
					return;
				}
				
				if(this.healthCodeIndex == -1){
					uni.showToast({
						icon: 'error',
						title: '请输入健康码状态',
					});
					return;
				}
				
				if(this.trip == ''){
					uni.showToast({
						icon: 'error',
						title: '请输入最近行程',
					});
					return;
				}
				
				if(this.registrationTime == ''){
					uni.showToast({
						icon: 'error',
						title: '请输入来返日期',
					});
					return;
				}
				
				if(this.photo == ''){
					uni.showToast({
						icon: 'error',
						title: '请上传人脸照片',
					});
					return;
				}
				
				if(this.nucleicAcid == ''){
					uni.showToast({
						icon: 'error',
						title: '请上传核酸检测报告',
					});
					return;
				}
				
				console.log('人脸数组',this.photo)
				uni.request({
					url:"http://123.156.228.92:20443/system/manage/update",
					method:"POST", //method：请求方法，少量信息用get，大量数据、加密类用post
					data:{
						name: this.name,  //姓名
						mobile: this.mobile, //手机号码
						idNum: this.idNum,
						nationValue: this.nationValue, //民族
						nativePlace: this.nativePlace, //户口所在地
						residencePlace: this.residencePlace, //现居住地址
						inoculation: this.inoculation, //接种次数
						healthCode: this.healthCode, //健康码状态
						trip: this.trip ,//行程
						registrationTime: this.registrationTime ,//返回时间
						photo: this.photo,
						nucleicAcid: this.nucleicAcid,
						deptid: this.deptId

					},
					// header:{ //header：请求头信息
					//  "content-type":"application/json"
					// },
					success:function(res){ //success:请求数据成功后的返回
						console.log(res,res.data.code)
						if(res.data.code == 200){
							uni.navigateTo({
								url: './successMsg'
							})
						}else if(res.data.code == 500){
							uni.showToast({
								icon: 'none',
								title: res.data.msg,
								duration: 2000
							});
						}
					}
				})
				
				// 获取文本框里的内容进行缓存
				
					
				uni.setStorage({
					key: 'demo_key',
					data: {
						'name': this.name,  //姓名
						'mobile': this.mobile, //手机号码
						'idNum': this.idNum,
						'nationArrIndex': this.nationArrIndex, //民族索引
						'nativePlace': this.nativePlace, //户口所在地
						'residencePlace': this.residencePlace, //现居住地址
						'inoculation': this.inoculation, //接种次数
						'healthCodeIndex': this.healthCodeIndex, //健康码状态索引
						'trip': this.trip ,//行程
						'registrationTime': this.registrationTime ,//返回时间
						'photo': this.photo,
						'nucleicAcid': this.nucleicAcid,
						'deptid': this.deptId,
					},
					success: function () {
						console.log('success');
					}
				});
				
				this.submit_SMS()
				
			},
			
			submitTest(){
				console.log('6545465',this.submit_title)
				if(this.submit_title != '提交'){
					uni.showToast({
						title: `${this.submit_title}`,
						icon: 'error'
					});
					return;
				}
			},
			
			
			
			// base64转blob
			pngBase64ToBlob(urlData) {
			    try {
			        var arr = urlData.split(',')
			        var mime = arr[0].match(/:(.*?);/)[1] || 'image/png';
			        // 去掉url的头，并转化为byte
			        var bytes = window.atob(arr[1]);
			        // 处理异常,将ascii码小于0的转换为大于0
			        var ab = new ArrayBuffer(bytes.length);
			        // 生成视图（直接针对内存）：8位无符号整数，长度1个字节
			        var ia = new Uint8Array(ab);
			        
			        for (var i = 0; i < bytes.length; i++) {
			            ia[i] = bytes.charCodeAt(i);
			        }
			 
			        return new Blob([ab], {
			            type: mime
			        });
			    }
			    catch (e) {
			        var ab = new ArrayBuffer(0);
			        return new Blob([ab], {
			            type: 'image/png',
			        });
			    }
			},
			
			// blob转本地url：
			
			blobToUrl(blob_data) {
			    return URL.createObjectURL(blob_data)
			},
			
			/*防止图片多次提交
			photo_SMS: function (e) {
				let timer1 = setInterval(() => {
					this.photo_timer--;
					this.photo_title='请等待'+this.photo_timer+'秒后再次上传人脸'
					console.log('人脸照片---',this.photo_timer)
					if (this.photo_timer == 0) {
						clearInterval(timer1);
						this.photo_timer = 60;
						this.photo_title='人脸照片'
						return;
					}
				}, 1000);
			},
			
			nucleicAcid_SMS: function (e) {
				let timer1 = setInterval(() => {
					this.timer--;
					this.title='请等待'+this.timer+'秒后再次上传核酸报告'
					console.log('核酸检测倒计时---',this.title)
					if (this.timer == 0) {
						clearInterval(timer1);
						this.timer = 60;
						this.title='核酸检测报告'
						return;
					}
				}, 1000);
			},*/
			
			submit_SMS: function () {
				// 对时间进行缓存
				var date = new Date();
				var dateH = date.getHours()
				var surplusH = 24-dateH;
				var shengxiaH = 86400-dateH*60*60
				this.shengyuH = shengxiaH/60/60
				this.submitIF = false;
				this.remainTime = date.getDate();
				console.log(dateH)
				this.submit_title='今日提交次数上限';
				uni.setStorage({
					key: 'submit_time',
					data: `${this.submit_title},${this.submitIF},${this.remainTime}`,
					success: function () {
						console.log('success');
					}
				})
			},
			
			
			
			
		}
	}
</script>

<style>
	page{
		background: #fff;
	}
	
	.visitorsBox{
		width: 92%;
		margin: 0 auto;
	}
	
	.visitorsTitle{
		margin: 30rpx;
		font-size: 40rpx;
		font-weight: bold;
	}
	
	.visitorsTitleContent{
		width: 85%;
		margin: 0 auto;
	}
	
	.idCard{
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
		margin-bottom: 40rpx;
	}
	
	.idCardLeft{
		width: 32%;
		font-size: 26rpx;
		line-height: 62rpx;
	}
	
	.idCardTips{
		position: relative;
	}
	
	.idCardTips>image{
		width: 40rpx;
		height: 40rpx;
		margin-top: 10rpx;
		margin-left: 10rpx;
	}
	
	.idCardTipsImg{
		position: absolute;
		z-index:1;
		background-color: #fff;
		left: -420rpx;
		top: 88rpx;
		box-shadow: 0 2px 15px #878282
	}
	
	.idCardTipsImg image{
		width: 520rpx;
		height: 592rpx;
	}
	
	.ownmsgNameRight{
		line-height: 60rpx;
		text-indent: 20rpx;
		
		font-size: 28rpx;
		color:grey;
	}
	
	.startColor{
		color:red;
		margin-right: 10rpx;
	}
	
	.visitorsListName{
		font-weight: bold;
	}
	
	.idCardContent{
		flex: 1;
		border: 1px solid #c7ccd3;
		border-radius: 10rpx;
	}
	
	.inputColor{
		height: 100%;
		font-size: 28rpx;
		color:grey;
		text-indent: 20rpx;
	}
	
	.photoText{
		width: 100%;
		text-align: center;
		font-size: 24rpx;
	}
	
	.photoText text{
		color:red;
	}
	
	.submitButton{
		margin-top: 40rpx;
		padding-bottom: 40rpx;
	}
	
	.submitButton button{
		background-color: #1b8af6;
		color:#fff;
		border-radius: 50rpx;
	}
	
	
	
	
	
</style>