<template>
    <div class="daily">
        <div class="daily-menu">
            <div class="daily-menu-item"  :class="{on:type==='recommend'}">每日推荐</div>
            <div class="daily-menu-item" :class="{on:type==='daily'}" @click="showThemes=!showThemes">主题日报</div>
            <ul class="showThemes">
                <li v-for="item in themes">
                    <a :class="{on: item.id===themeId&&type==='daily'}">{{item.name}}</a>
                </li>
            </ul>
        </div>
        <div class="daily-list">
           <template v-if="type==='recommend'">
               <div v-for="list in recommendList">
                   <div class="daily-date">{{formaDay(list.date)}}</div>
                   <Item v-for="item in list.stories" :data="item" :key="item.id" @click.native="handleClick(item.id)"></Item>
               </div>
           </template>
           <template>

           </template>
        </div>
        <daily-article></daily-article>
    </div>
</template>
<script>
import $ from './libs/util.js';
export default {
    data(){
        return {
            themes: [],
            showThemes: false,
            type: 'recommend',
            recommendList: [],
            dailyTime: $.getTodayTime(),
            list: [],
            themeId: 0,
            articleId: 0,
            isLoading: false
        }
    },
    methods:{
        getThemes(){
            //axios发起get请求
            $.ajax.get('themes').then(res=>{this.themes=res.others;})
        },
        handleToTheme(id){
            //改变菜单分类
            this.type='daily';
            //设置当前点击子类的主题日报ID
            this.themeId=id;
            this.list[];
            $.ajax.get('theme/'+id).then(res=>{
                this.list=res.stories.filter(item=>item.type!==1);
            })
        },
        handleToRecommend(){
            this.type='recommend';
            thi.recommendList=[];
            this.dailyTime=$.getTodayTime();
            this.getRecommendList();
        },
        getRecommendList(){
            this.isLoading=true;
            const prevDay=$.prevDay(this.dailyTime+86400000);
            $.ajax.get('news/before/'+prevDay).then(res=>{
                this.recommendList.push(res);
                this.isLoading=false;
            })
        }
    },
    mounted(){
        //初始化时调用
        this.getThemes();
        this.getRecommendList();
    }
}
</script>

