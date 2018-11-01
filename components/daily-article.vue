<template>
    <div class="daily">
        <Item @click.native="handleClick(item.id)"></Item>
        <daily-article :id="articleId"></daily-article>        
    </div>
</template>
<template>
    <div class="daily-artice">
        <div class="daily-article-title">{{data.title}}</div>
        <div class="daily-article-content" v-html="data.body"></div>
    </div>
</template>

<script>
import Item from '../components/item';
import dailyArticle from '../components/daily-article';
import $ from '../libs/util.js';
export default {
    props:{
        id:{
            type:Number,
            default:0
        }
    },
    components:{Item,dailyArticle},
    data(){
        return {
            articleId:0,
            data:{}
        }
    },
    methods:{
        handleClick(id){
            this.articleId=id;
        },
        getArticle(){
            $.ajax.get('news/'+this.id).then(res=>{
                //将文章的图片地址替换为代理地址
                res.body=res.body.replace(/src="http/g,'src="'+$.imgPath+'http');
                res.body=res.body.replace(/src="http/g,'src="'+$.imgPath+'https');
                this.data=res;
                //回到文章顶端
                window.scrollTo(0,0);
            })
        }
    },
    watch:{
        id(val){
            if (val) {
                this.getArticle();
            }
        }
    }
}
</script>

