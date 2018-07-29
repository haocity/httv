<template>
    <div class="ht-tv" :class="{'ht-isfull':isfull}">
        <div class="ht-tv-r" :style="{height: h+'px'}" v-show="showright">
			<div class="ht-tv-r-w"  v-for='(item, index) in list' >
				<div class="ht-tv-r-t" @click="showlist(index)">{{item.class}}</div>
				<div class="ht-tv-r-i" v-show="item.show" v-for='tv in item.list' @click.stop='htplay(tv,index)' :class="{active:(tv.id==nowi)&&(nowlist==item.lid)}">
					{{tv.name}}
				</div>
			</div>
        </div>

        <div class="ht-tv-m" :style="{width:w-300+'px',height: h+'px'}">
            <video id="video" ref="video"></video>
            <div class="ht-tv-b">
                <div class="ht-btn iconfont icon-play " :class="{ 'icon-zanting' : isplay }" @click='swichplay'></div>
                <div class="ht-btn iconfont icon-quanping ht-full" @click='swichfull'></div>
            </div>
        </div>
    </div>
</template>

<script>
	 import Hls from 'hls.js';
	
    let hls = new Hls();

    export default {
        name: 'ht-tv-page',

        data() {
            return {
                w: '0',
                h: '0',
				nowlist:0,
                nowi: null,
                isplay: false,
                isfull: false,
                showright: true,
                list: [{
                    lid: 0,
                    class: '央视',
					
                    list: [{
                            id: 0,
                            name: 'CCTV-1',
                            url: ['http://223.110.241.204:6610/gitv/live1/G_CCTV-1-HQ/G_CCTV-1-HQ/']
                        },
                        {
                            id: 1,
                            name: 'CCTV-2',
                           
                            url: ['http://223.110.241.204:6610/gitv/live1/G_CCTV-2-HD/G_CCTV-2-HD/']
                        },
                        {
                            id: 2,
                            name: 'CCTV-3',
                           
                            url: ['http://223.110.241.204:6610/gitv/live1/G_CCTV-3-HQ/G_CCTV-3-HQ/']
                        },
                        {
                            id: 3,
                            name: 'CCTV-4',
                           
                            url: ['http://223.110.241.204:6610/gitv/live1/G_CCTV-4-HQ/G_CCTV-4-HQ/']
                        },
                        {
                            id: 4,
                            name: 'CCTV-5',
                            
                            url: ['http://223.110.241.204:6610/gitv/live1/G_CCTV-5-HQ/G_CCTV-5-HQ/']
                        },
                        {
                            id: 5,
                            name: 'CCTV-6',
                       
                            url: ['http://223.110.241.204:6610/gitv/live1/G_CCTV-6-HQ/G_CCTV-6-HQ/']
                        },
                        {
                            id: 6,
                            name: 'CCTV-7',
                         
                            url: ['http://223.110.241.204:6610/gitv/live1/G_CCTV-7-HQ/G_CCTV-7-HQ/']
                        },
                        {
                            id: 7,
                            name: 'CCTV-8',
                      
                            url: ['http://223.110.241.204:6610/gitv/live1/G_CCTV-8-HQ/G_CCTV-8-HQ/']
                        },
                        {
                            id: 8,
                            name: 'CCTV-9',
                        
                            url: ['http://223.110.241.204:6610/gitv/live1/G_CCTV-9/G_CCTV-9/']
                        },
                        {
                            id: 9,
                            name: 'CCTV-10',
                          
                            url: ['http://223.110.241.204:6610/gitv/live1/G_CCTV-10-HQ/G_CCTV-10-HQ/']
                        },
                        {
                            id: 10,
                            name: 'CCTV-11',
                         
                            url: ['http://223.110.241.204:6610/gitv/live1/G_CCTV-11-HQ/G_CCTV-11-HQ/']
                        },
                        {
                            id: 11,
                            name: 'CCTV-12',
                          
                            url: ['http://223.110.241.204:6610/gitv/live1/CCTV-12/CCTV-12/']
                        },
                        {
                            id: 12,
                            name: 'CCTV-13 新闻',
                            
                            url: ['http://223.110.241.204:6610/gitv/live1/G_CCTV-13-HQ/G_CCTV-13-HQ/']
                        },
                        {
                            id: 13,
                            name: 'CCTV-14 少儿',
                           
                            url: ['http://223.110.241.204:6610/gitv/live1/G_CCTV-14/G_CCTV-14/']
                        }, {
                            id: 14,
                            name: 'CCTV-15 音乐',
                            
                            url: ['http://223.110.241.204:6610/gitv/live1/G_CCTV-15/G_CCTV-15/']
                        }
                    ]
                },{
                    lid: 1,
                    class: '山东',
                    list: [{
                            id: 0,
                            name: '山东卫视',
                            url: ['http://ksdlive.iqilu.com/sdtv.m3u8']
                        },{
                            id: 1,
                            name: '山东齐鲁',
                            url: ['http://ksdlive.iqilu.com/live2/qlpd.m3u8']
                        },{
                            id: 2,
                            name: '济南影视',
                            url: ['http://ts1.ijntv.cn/yshd/hd/live.m3u8']
                        },{
                            id: 3,
                            name: '济南娱乐',
                            url: ['http://ts2.ijntv.cn/jnyl/sd/live.m3u8']
                        },{
                            id: 4,
                            name: '济南生活',
                            url: ['http://ts2.ijntv.cn/jnse/sd1/live.m3u8']
                        },{
                            id: 5,
                            name: '济南少儿',
                            url: ['http://ts1.ijntv.cn/jnsw/sd/live.m3u8']
                        }
                    ]
                },{
                    lid: 2,
                    class: '山东IPTV源',
                    list: [{"id":0,"name":"浙江卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000305/index.m3u8"]},{"id":1,"name":"黑龙江高清","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000306/index.m3u8"]},{"id":2,"name":"深圳卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000308/index.m3u8"]},{"id":3,"name":"怀旧剧场","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000622/index.m3u8"]},{"id":4,"name":"","url":[null]},{"id":5,"name":"CCTV-1","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000001/index.m3u8"]},{"id":6,"name":"山东卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000051/index.m3u8"]},{"id":7,"name":"CCTV-1高清","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000269/index.m3u8"]},{"id":8,"name":"齐鲁高清","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000313/index.m3u8"]},{"id":9,"name":"山东影视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000056/index.m3u8"]},{"id":10,"name":"山东生活","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000055/index.m3u8"]},{"id":11,"name":"山东综艺","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000054/index.m3u8"]},{"id":12,"name":"山东体育","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000057/index.m3u8"]},{"id":13,"name":"山东公共","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000053/index.m3u8"]},{"id":14,"name":"山东农科","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000058/index.m3u8"]},{"id":15,"name":"山东少儿","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000059/index.m3u8"]},{"id":16,"name":"山东国际","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000060/index.m3u8"]},{"id":17,"name":"山东购物","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000700/index.m3u8"]},{"id":18,"name":"CCTV-2","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000002/index.m3u8"]},{"id":19,"name":"CCTV-3","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000003/index.m3u8"]},{"id":20,"name":"CCTV-4","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000004/index.m3u8"]},{"id":21,"name":"CCTV-5","url":["http://39.137.76.32:8089/00/SNM/CHANNEL30000005/index.m3u8"]},{"id":22,"name":"CCTV-6","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000006/index.m3u8"]},{"id":23,"name":"CCTV-7","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000007/index.m3u8"]},{"id":24,"name":"CCTV-8","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000008/index.m3u8"]},{"id":25,"name":"CCTV-9","url":["http://39.137.76.32:8089/00/SNM/CHANNEL10000009/index.m3u8"]},{"id":26,"name":"CCTV-10","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000010/index.m3u8"]},{"id":27,"name":"CCTV-11","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000011/index.m3u8"]},{"id":28,"name":"CCTV-12","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000012/index.m3u8"]},{"id":29,"name":"CCTV-13","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000013/index.m3u8"]},{"id":30,"name":"CCTV-14","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000014/index.m3u8"]},{"id":31,"name":"CCTV-15","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000015/index.m3u8"]},{"id":32,"name":"湖南卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000017/index.m3u8"]},{"id":33,"name":"浙江卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000016/index.m3u8"]},{"id":34,"name":"江苏卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000018/index.m3u8"]},{"id":35,"name":"安徽卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000020/index.m3u8"]},{"id":36,"name":"北京卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000019/index.m3u8"]},{"id":37,"name":"东方卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000022/index.m3u8"]},{"id":38,"name":"深圳卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000024/index.m3u8"]},{"id":39,"name":"天津卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000021/index.m3u8"]},{"id":40,"name":"旅游卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000030/index.m3u8"]},{"id":41,"name":"辽宁卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000026/index.m3u8"]},{"id":42,"name":"黑龙江卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000027/index.m3u8"]},{"id":43,"name":"重庆卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000023/index.m3u8"]},{"id":44,"name":"广东卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000025/index.m3u8"]},{"id":45,"name":"广西卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000038/index.m3u8"]},{"id":46,"name":"东南卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000028/index.m3u8"]},{"id":47,"name":"云南卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000039/index.m3u8"]},{"id":48,"name":"河北卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000034/index.m3u8"]},{"id":49,"name":"湖北卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000031/index.m3u8"]},{"id":50,"name":"吉林卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000036/index.m3u8"]},{"id":51,"name":"江西卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000035/index.m3u8"]},{"id":52,"name":"山西卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000032/index.m3u8"]},{"id":53,"name":"陕西卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000029/index.m3u8"]},{"id":54,"name":"四川卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000033/index.m3u8"]},{"id":55,"name":"贵州卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000041/index.m3u8"]},{"id":56,"name":"甘肃卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000046/index.m3u8"]},{"id":57,"name":"宁夏卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000043/index.m3u8"]},{"id":58,"name":"青海卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000042/index.m3u8"]},{"id":59,"name":"西藏卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000045/index.m3u8"]},{"id":60,"name":"新疆卫视","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000044/index.m3u8"]},{"id":61,"name":"内蒙古","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000040/index.m3u8"]},{"id":62,"name":"金鹰卡通","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000047/index.m3u8"]},{"id":63,"name":"卡酷动画","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000048/index.m3u8"]},{"id":64,"name":"优漫卡通","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000049/index.m3u8"]},{"id":65,"name":"炫动卡通","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000050/index.m3u8"]},{"id":66,"name":"CCTV-NEWS","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000009/index.m3u8"]},{"id":67,"name":"山东教育","url":["http://39.137.76.32:8089/00/SNM/CHANNEL10000062/index.m3u8"]},{"id":68,"name":"泰安公共","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000270/index.m3u8"]},{"id":69,"name":"山东高清","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000301/index.m3u8"]},{"id":70,"name":"北京卫视高清","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000302/index.m3u8"]},{"id":71,"name":"江苏卫视高清","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000304/index.m3u8"]},{"id":72,"name":"广东卫视高清","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000307/index.m3u8"]},{"id":73,"name":"深圳卫视高清","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000308/index.m3u8"]},{"id":74,"name":"","url":[null]},{"id":75,"name":"东方卫视高清","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000309/index.m3u8"]},{"id":76,"name":"东南卫视高清","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000310/index.m3u8"]},{"id":77,"name":"山东齐鲁","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000052/index.m3u8"]},{"id":78,"name":"百姓健康","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000602/index.m3u8"]},{"id":79,"name":"央视健康","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000603/index.m3u8"]},{"id":80,"name":"美食天府","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000604/index.m3u8"]},{"id":81,"name":"家庭理财","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000605/index.m3u8"]},{"id":82,"name":"证券资讯","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000606/index.m3u8"]},{"id":83,"name":"财富天下","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000607/index.m3u8"]},{"id":84,"name":"休闲指南","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000608/index.m3u8"]},{"id":85,"name":"中国天气","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000609/index.m3u8"]},{"id":86,"name":"家政","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000610/index.m3u8"]},{"id":87,"name":"电子体育","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000611/index.m3u8"]},{"id":88,"name":"人物频道","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000612/index.m3u8"]},{"id":89,"name":"风云音乐","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000613/index.m3u8"]},{"id":90,"name":"第一剧场","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000614/index.m3u8"]},{"id":91,"name":"世界地理","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000615/index.m3u8"]},{"id":92,"name":"女性时尚","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000616/index.m3u8"]},{"id":93,"name":"风云剧场","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000617/index.m3u8"]},{"id":94,"name":"风云足球","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000618/index.m3u8"]},{"id":95,"name":"高尔夫网球","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000620/index.m3u8"]},{"id":96,"name":"央视文化","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000621/index.m3u8"]},{"id":97,"name":"怀旧剧场","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000622/index.m3u8"]},{"id":98,"name":"老故事","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000624/index.m3u8"]},{"id":99,"name":"新科动漫","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000625/index.m3u8"]},{"id":100,"name":"早期教育","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000627/index.m3u8"]},{"id":101,"name":"环球奇观","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000628/index.m3u8"]},{"id":102,"name":"现代女性","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000629/index.m3u8"]},{"id":103,"name":"武术世界","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000634/index.m3u8"]},{"id":104,"name":"文物宝库","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000635/index.m3u8"]},{"id":105,"name":"国学频道","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000636/index.m3u8"]},{"id":106,"name":"彩民在线","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000638/index.m3u8"]},{"id":107,"name":"卫生健康","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000639/index.m3u8"]},{"id":108,"name":"靓状","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000406/index.m3u8"]},{"id":109,"name":"看海电竞","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000408/index.m3u8"]},{"id":110,"name":"看海生活","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000202/index.m3u8"]},{"id":111,"name":"看海教育","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000204/index.m3u8"]},{"id":112,"name":"看海演艺","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000400/index.m3u8"]},{"id":113,"name":"看海健康","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000401/index.m3u8"]},{"id":114,"name":"看海少儿","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000402/index.m3u8"]},{"id":115,"name":"看海剧场","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000403/index.m3u8"]},{"id":116,"name":"看海大片","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000407/index.m3u8"]},{"id":117,"name":"看海热播","url":["http://39.137.76.32:8089/00/SNM/CHANNEL00000405/index.m3u8"]}]
                },{
                    lid: 3,
                    class: '测试频道',
                    list: [{"id":0,"name":"民視HD","url":["http://tv.two3.cn/Tv/L2tv?id=1"]},{"id":1,"name":"台視HD","url":["http://tv.two3.cn/Tv/L2tv?id=2"]},{"id":2,"name":"中視HD","url":["http://tv.two3.cn/Tv/L2tv?id=3"]},{"id":3,"name":"華視HD","url":["http://tv.two3.cn/Tv/L2tv?id=4"]},{"id":4,"name":"公視HD","url":["http://tv.two3.cn/Tv/L2tv?id=5"]},{"id":5,"name":"公視2","url":["http://tv.two3.cn/Tv/L2tv?id=6"]},{"id":6,"name":"公視3","url":["http://tv.two3.cn/Tv/L2tv?id=7"]},{"id":7,"name":"大愛","url":["http://tv.two3.cn/Tv/L2tv?id=8"]},{"id":8,"name":"原住民電視台","url":["http://tv.two3.cn/Tv/L2tv?id=9"]},{"id":9,"name":"客家電視台","url":["http://tv.two3.cn/Tv/L2tv?id=10"]},{"id":10,"name":"民視第一台","url":["http://tv.two3.cn/Tv/L2tv?id=11"]},{"id":11,"name":"民視台灣台","url":["http://tv.two3.cn/Tv/L2tv?id=12"]},{"id":12,"name":"華視新聞","url":["http://tv.two3.cn/Tv/L2tv?id=13"]},{"id":13,"name":"年代新聞台","url":["http://tv.two3.cn/Tv/L2tv?id=14"]},{"id":14,"name":"非凡新聞台","url":["http://tv.two3.cn/Tv/L2tv?id=15"]},{"id":15,"name":"民視新聞台","url":["http://tv.two3.cn/Tv/L2tv?id=16"]},{"id":16,"name":"三立新聞","url":["http://tv.two3.cn/Tv/L2tv?id=17"]},{"id":17,"name":"中天新聞台","url":["http://tv.two3.cn/Tv/L2tv?id=18"]},{"id":18,"name":"TVBS新聞台","url":["http://tv.two3.cn/Tv/L2tv?id=19"]},{"id":19,"name":"台視財經台","url":["http://tv.two3.cn/Tv/L2tv?id=20"]},{"id":20,"name":"台視新聞","url":["http://tv.two3.cn/Tv/L2tv?id=21"]},{"id":21,"name":"中視新聞","url":["http://tv.two3.cn/Tv/L2tv?id=22"]},{"id":22,"name":"Disney","url":["http://tv.two3.cn/Tv/L2tv?id=23"]},{"id":23,"name":"Boomerang卡通台","url":["http://tv.two3.cn/Tv/L2tv?id=24"]},{"id":24,"name":"ANIMAX","url":["http://tv.two3.cn/Tv/L2tv?id=25"]},{"id":25,"name":"Discovery","url":["http://tv.two3.cn/Tv/L2tv?id=26"]},{"id":26,"name":"動物星球","url":["http://tv.two3.cn/Tv/L2tv?id=27"]},{"id":27,"name":"亞洲美食","url":["http://tv.two3.cn/Tv/L2tv?id=28"]},{"id":28,"name":"HGTV","url":["http://tv.two3.cn/Tv/L2tv?id=29"]},{"id":29,"name":"TLC","url":["http://tv.two3.cn/Tv/L2tv?id=30"]},{"id":30,"name":"DMAX","url":["http://tv.two3.cn/Tv/L2tv?id=31"]},{"id":31,"name":"國家地理HD","url":["http://tv.two3.cn/Tv/L2tv?id=32"]},{"id":32,"name":"美食網","url":["http://tv.two3.cn/Tv/L2tv?id=33"]},{"id":33,"name":"MOMO","url":["http://tv.two3.cn/Tv/L2tv?id=34"]},{"id":34,"name":"Discovery亞洲","url":["http://tv.two3.cn/Tv/L2tv?id=35"]},{"id":35,"name":"FashionTV","url":["http://tv.two3.cn/Tv/L2tv?id=36"]},{"id":36,"name":"GTV八大娛樂台","url":["http://tv.two3.cn/Tv/L2tv?id=37"]},{"id":37,"name":"GTV八大第一台","url":["http://tv.two3.cn/Tv/L2tv?id=38"]},{"id":38,"name":"GTV八大綜合台","url":["http://tv.two3.cn/Tv/L2tv?id=39"]},{"id":39,"name":"GTV八大戲劇台","url":["http://tv.two3.cn/Tv/L2tv?id=40"]},{"id":40,"name":"三立台灣台","url":["http://tv.two3.cn/Tv/L2tv?id=41"]},{"id":41,"name":"霹靂台灣台","url":["http://tv.two3.cn/Tv/L2tv?id=42"]},{"id":42,"name":"緯來日本","url":["http://tv.two3.cn/Tv/L2tv?id=43"]},{"id":43,"name":"高點綜合","url":["http://tv.two3.cn/Tv/L2tv?id=44"]},{"id":44,"name":"中天綜合台","url":["http://tv.two3.cn/Tv/L2tv?id=45"]},{"id":45,"name":"中天娛樂台","url":["http://tv.two3.cn/Tv/L2tv?id=46"]},{"id":46,"name":"KMTV","url":["http://tv.two3.cn/Tv/L2tv?id=47"]},{"id":47,"name":"國興衛視","url":["http://tv.two3.cn/Tv/L2tv?id=48"]},{"id":48,"name":"中視經典台","url":["http://tv.two3.cn/Tv/L2tv?id=49"]},{"id":49,"name":"中視菁采台","url":["http://tv.two3.cn/Tv/L2tv?id=50"]},{"id":50,"name":"台視綜合台","url":["http://tv.two3.cn/Tv/L2tv?id=51"]},{"id":51,"name":"華納TV","url":["http://tv.two3.cn/Tv/L2tv?id=52"]},{"id":52,"name":"EVE","url":["http://tv.two3.cn/Tv/L2tv?id=53"]},{"id":53,"name":"星空電影台","url":["http://tv.two3.cn/Tv/L2tv?id=54"]},{"id":54,"name":"LSTIME龍祥電影","url":["http://tv.two3.cn/Tv/L2tv?id=55"]},{"id":55,"name":"AXN","url":["http://tv.two3.cn/Tv/L2tv?id=56"]},{"id":56,"name":"FOXHD","url":["http://tv.two3.cn/Tv/L2tv?id=57"]},{"id":57,"name":"FOXSport3HD","url":["http://tv.two3.cn/Tv/L2tv?id=58"]},{"id":58,"name":"愛爾達體育","url":["http://tv.two3.cn/Tv/L2tv?id=59"]},{"id":59,"name":"Z頻道","url":["http://tv.two3.cn/Tv/L2tv?id=60"]},{"id":60,"name":"緯來體育","url":["http://tv.two3.cn/Tv/L2tv?id=61"]}]
					
								},{
									lid:4,
									class:'省级卫视',
									list:[{"id":0,"name":"湖南卫视超清","url":["http://223.110.241.204:6610/gitv/live1/G_HUNAN-CQ/G_HUNAN-CQ"]},{"id":1,"name":"江苏卫视超清","url":["http://223.110.241.204:6610/gitv/live1/G_JIANGSU-CQ/G_JIANGSU-CQ"]},{"id":2,"name":"北京卫视超清","url":["http://223.110.241.204:6610/gitv/live1/G_BEIJING-CQ/G_BEIJING-CQ"]},{"id":3,"name":"浙江卫视超清","url":["http://223.110.241.204:6610/gitv/live1/G_ZHEJIANG-CQ/G_ZHEJIANG-CQ"]},{"id":4,"name":"东方卫视超清","url":["http://223.110.241.204:6610/gitv/live1/G_DONGFANG-CQ/G_DONGFANG-CQ"]},{"id":5,"name":"安徽卫视","url":["http://223.110.241.204:6610/gitv/live1/AHWS/AHWS"]},{"id":6,"name":"辽宁卫视","url":["http://223.110.241.204:6610/gitv/live1/LNWS/LNWS"]},{"id":7,"name":"山东卫视","url":["http://223.110.241.204:6610/gitv/live1/SDWS/SDWS"]},{"id":8,"name":"广西卫视","url":["http://223.110.241.204:6610/gitv/live1/GXWS/GXWS"]},{"id":9,"name":"旅游卫视","url":["http://223.110.241.204:6610/gitv/live1/LYWS/LYWS"]},{"id":10,"name":"重庆卫视","url":["http://223.110.241.204:6610/gitv/live1/G_CHONGQING/G_CHONGQING"]},{"id":11,"name":"四川卫视","url":["http://223.110.241.204:6610/gitv/live1/SCWS/SCWS"]},{"id":12,"name":"河南卫视","url":["http://223.110.241.204:6610/gitv/live1/HENAN/HENAN"]},{"id":13,"name":"东南卫视","url":["http://223.110.241.204:6610/gitv/live1/DNWS/DNWS"]},{"id":14,"name":"天津卫视","url":["http://223.110.241.204:6610/gitv/live1/TJWS/TJWS"]},{"id":15,"name":"江西卫视","url":["http://223.110.241.204:6610/gitv/live1/JXWS/JXWS"]},{"id":16,"name":"湖北卫视","url":["http://223.110.241.204:6610/gitv/live1/G_HUBEI/G_HUBEI"]},{"id":17,"name":"吉林卫视","url":["http://223.110.241.204:6610/gitv/live1/G_JILIN/G_JILIN"]},{"id":18,"name":"山西卫视","url":["http://223.110.241.204:6610/gitv/live1/SXWS/SXWS"]},{"id":19,"name":"贵州卫视","url":["http://223.110.241.204:6610/gitv/live1/G_GUIZHOU/G_GUIZHOU"]},{"id":20,"name":"青海卫视","url":["http://223.110.241.204:6610/gitv/live1/G_QINGHAI/G_QINGHAI"]},{"id":21,"name":"内蒙古卫视","url":["http://223.110.241.204:6610/gitv/live1/G_NEIMENGGU/G_NEIMENGGU"]},{"id":22,"name":"云南卫视","url":["http://223.110.241.204:6610/gitv/live1/G_YUNNAN/G_YUNNAN"]},{"id":23,"name":"河北卫视","url":["http://223.110.241.204:6610/gitv/live1/G_HEBEI/G_HEBEI"]},{"id":24,"name":"宁夏卫视","url":["http://223.110.241.204:6610/gitv/live1/G_NINGXIA/G_NINGXIA"]},{"id":25,"name":"新疆卫视","url":["http://223.110.241.204:6610/gitv/live1/G_XINJIANG/G_XINJIANG"]},{"id":26,"name":"甘肃卫视","url":["http://223.110.241.204:6610/gitv/live1/G_GANSU/G_GANSU"]},{"id":27,"name":"广东卫视","url":["http://223.110.241.204:6610/gitv/live1/G_GUANGDONG/G_GUANGDONG"]},{"id":28,"name":"深圳卫视","url":["http://223.110.241.204:6610/gitv/live1/G_SHENZHEN/G_SHENZHEN"]},{"id":29,"name":"黑龙江卫视","url":["http://223.110.241.204:6610/gitv/live1/G_HEILONGJIANG/G_HEILONGJIANG"]},{"id":30,"name":"南方卫视广东","url":["http://223.110.241.204:6610/gitv/live1/G_NANFANG/G_NANFANG"]},{"id":31,"name":"厦门卫视,","url":["http://223.110.241.204:6610/gitv/live1/G_XIAMEN/G_XIAMEN"]},{"id":32,"name":"山东卫视超清","url":["http://223.110.241.204:6610/gitv/live1/G_SHANDONG-HQ/G_SHANDONG-HQ"]},{"id":33,"name":"嘉佳卡通","url":["http://223.110.241.204:6610/gitv/live1/G_JIAJIA/G_JIAJIA"]},{"id":34,"name":"优漫卡通","url":["http://223.110.241.204:6610/gitv/live1/G_YOUMAN/G_YOUMAN"]}]
								}
								
						]
            }
        },
        methods: {
            htplay(item,a) {
                console.log(item,a)
				this.nowlist=a
                this.nowi = item.id
                let video = this.$refs.video
                hls.loadSource(item.url[0]);
                hls.attachMedia(video);
                hls.on(Hls.Events.MANIFEST_PARSED, () => {
                    video.play();
                    this.isplay = true
                });
            },
            swichplay() {
                if (this.nowi === null) {
                    this.htplay(this.list[0].list[0])
                } else {
                    if (this.isplay) {
                        this.$refs.video.pause()
                    } else {
                        this.$refs.video.play();
                    }
                    this.isplay = !this.isplay
                }
            },
            swichfull() {
                let e = this.$el
                if (document.fullscreenElement || document.webkitFullscreenElement) {
                    if (document.cancelFullScreen) {
                        document.cancelFullScreen()
                    } else if (document.webkitCancelFullScreen) {
                        document.webkitCancelFullScreen()
                    }

                } else {
                    if (e.requestFullscreen) {
                        e.requestFullscreen()
                    } else if (e.webkitRequestFullscreen) {
                        e.webkitRequestFullscreen()
                    }
                }
            },showlist(index){
				
				if(this.list[index].show){
					this.$set(this.list[index],'show',false)
				}else{
					this.$set(this.list[index],'show',true)
				}
				
			}

        },
        created() {
            this.w = window.innerWidth
            this.h = window.innerHeight
            document.body.onresize = () => {
                this.w = window.innerWidth
                this.h = window.innerHeight
            }

        },
        mounted() {
            let screenChange = 'webkitfullscreenchange' || 'fullscreenchange'
            this.$el.addEventListener(screenChange, () => {
                if (document.fullscreenElement || document.webkitFullscreenElement) {
                    this.isfull = true
                } else {
                    this.isfull = false
                }
            }, false);
        }
    }
</script>

<style>
    @font-face {
        font-family: "iconfont";
        src: url('//at.alicdn.com/t/font_648784_5dxjlcr5t63cjtt9.eot?t=1525077870094');
        /* IE9*/
        src: url('//at.alicdn.com/t/font_648784_5dxjlcr5t63cjtt9.eot?t=1525077870094#iefix') format('embedded-opentype'), /* IE6-IE8 */
        url('data:application/x-font-woff;charset=utf-8;base64,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') format('woff'),
        url('//at.alicdn.com/t/font_648784_5dxjlcr5t63cjtt9.ttf?t=1525077870094') format('truetype'), /* chrome, firefox, opera, Safari, Android, iOS 4.2+*/
        url('//at.alicdn.com/t/font_648784_5dxjlcr5t63cjtt9.svg?t=1525077870094#iconfont') format('svg');
        /* iOS 4.1- */
    }

    .iconfont {
        font-family: "iconfont" !important;
        font-size: 16px;
        font-style: normal;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
    }

    .icon-play:before {
        content: "\e865";
    }

    .icon-zanting:before {
        content: "\e67d";
    }

    .icon-xunhuan:before {
        content: "\e61b";
    }

    .icon-yinliang:before {
        content: "\e63f";
    }

    .icon-shezhi1:before {
        content: "\e633";
    }

    .icon-jingyin:before {
        content: "\e61d";
    }

    .icon-bofangqi-danmuguan:before {
        content: "\e696";
    }

    .icon-bofangqi-danmukai:before {
        content: "\e697";
    }

    .icon-quanping:before {
        content: "\e626";
    }


    * {
        margin: 0;
        padding: 0;
    }

    body {
        overflow: hidden;
		font-family: Avenir Next, Helvetica, Arial, Lantinghei SC, Microsoft YaHei, sans-serif;
    }

    #video {
        width: 100%;
        height: 100%;
    }

    .ht-tv {
        position: relative;
        width: 100%;
		height: 100%;
    }

    .ht-tv-m {

        background-color: #000;
    }

    .ht-tv-r {
        width: 300px;

        background-color: #f9f9f9;
        position: absolute;
        right: 0;
        top: 0;
        z-index: 999;

        overflow-y: scroll;
    }
	.ht-tv-r-t{
		padding-left: 20px;
		line-height: 46px;
		background-color: #35495E;
		color: #fff;
	}
    .ht-tv-r-i {
        padding: 0 36px;
        line-height: 50px;
        cursor: pointer;
    }

    .ht-tv-r-i:hover,
    .ht-tv-r-i.active {
        background-color: #c7c7c7;
    }

    .ht-tv-b {
        position: relative;
        bottom: 60px;
        left: 0;
        height: 54px;
        width: 100%;

        background-color: rgba(31, 40, 148, 0.8);
    }

    .ht-btn {
        line-height: 58px;
        font-size: 26px;
        color: #fff;
        width: 60px;
        text-align: center;
    }

    .ht-full {
        position: absolute;
        right: 10px;
        top: 0;
    }
	.ht-isfull .ht-tv-r{
		display: none!important;
		
	}
	.ht-isfull .ht-tv-m{
		width: 100%!important;
		height: 100%!important;
	}
	.ht-isfull .ht-tv-b{
		opacity: 0;
		transition: opacity 0.4s;
		
		height: 60px;
	}
	.ht-isfull .ht-tv-b:hover{
		opacity: 1;
	}
	
</style>
