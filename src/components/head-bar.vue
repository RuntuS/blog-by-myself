<template>
    <div id="head-bar"  :style="{height : clientHeight * 0.8 + 'px'}" >


        <Row class="row-1">
            <transition name="intro"  >
                <Col class="font-index" v-if="hasIntro" >LaoLan's Blog</Col>
            </transition>
            <transition name="wh">
                <Col class="waihao-index"   v-if="waihaoIntro">Runtus</Col>
            </transition>

        </Row>



        <Row   :class="(headBar || isPC) ? 'row-2' : 'row-2-go'">
            <Col class="col-1-for-router"  :xs="17"   :sm="{span : 13, offset : 3}">
                <Row style="height: 100%;"  type="flex">
                    <Col  class="router" :sm="5"  :xs="6"  ><div class="font" @click="jumpHome"><span>主页</span></div></Col>
                    <Col  class="router" :sm="5"  :xs="6"     ><div class="font"  @click="jumpAbout"><span>关于我</span></div></Col>
                    <Col  class="router" :sm="5"  :xs="6"   ><div class="font" @click="jumpClass"><span>分类</span></div></Col>
                </Row>
            </Col>

            <Col :sm="5" :xs="7" class="col-2">
                <a href="https://github.com/RuntuS" target="_blank" >
                    <Icon type="logo-github" size="35"   />
                </a>
            </Col>
        </Row>
    </div>
</template>

<script>
    export default {
        name: "head-bar",
        data(){
            return {
                clientHeight : window.screen.height,
                hasIntro : false,
                waihaoIntro :false,
                theLastY : 0,//记录上一次Y的距离
                thisY : 0,//新的Y
                headBar : true,
                isPC : false
            }
        },
        created() {
            this.$Message.success("你们好");
            if(window.screen.width <= 500)
            {
                this.isPC = false
            }
            else
            {
                this.isPC = true ;
            }
        },
        mounted() {
            setTimeout(() => {
                this.hasIntro = true;
            },500);

            window.addEventListener("scroll",this.handleScroll);
            window.addEventListener("scroll",this.changeHeadBar);
        },
        methods:{
            jumpHome(){
                console.log("你好");
                this.$router.push("/home/1");
            },
            jumpAbout(){
                this.$router.push("/about");
            },
            jumpClass(){
                this.$router.push("/class");
            },
            handleScroll(){

                if(window.scrollY >= 300)
                {
                    this.hasIntro = false;
                    this.waihaoIntro = true
                }
                else if(this.hasIntro === false && window.scrollY < 300)
                {
                    this.hasIntro = true;
                    this.waihaoIntro =false;
                }
            },

            changeHeadBar(){
                this.Y = window.pageYOffset;
                let sub = this.Y - this.theLastY;
                this.theLastY = window.pageYOffset;
                if(sub > 0)
                {
                    this.headBar = false;
                }
                else {
                    this.headBar = true;
                }
            }


        }

    }
</script>

<style scoped>
#head-bar{
    /*background-color: darkgray;*/

    position: relative;


}

.row-1{
    position: relative;
}

.font-index{
    position: absolute;
    font-size: 80px;
    color: white;
    font-weight: 800;
    left: 35%;
    top: 300px;

}
.row-2{
    width: 100%;
    height: 10%;
    /*border: 1px solid blue;*/
    position: absolute;
    bottom: 0;

}


.row-2-go{

}


.col-1-for-router{
    height: 100%;
    border-bottom-left-radius: 10px;
    border-top-left-radius: 10px;
    background-color: rgba(242,242,242,0.3);
}

.col-2{
    height: 100%;
    border-top-right-radius: 10px;
    border-bottom-right-radius: 10px;
    background-color: rgba(242,242,242,0.5);
    padding-top: 1%;
    padding-left: 4%;

}

.router{
    position: relative;
    border-radius: 5px;

    height: 100%;
    text-align: center;
    color: white;

    font-family: "Source Sans Pro", Helvetica, sans-serif;
    font-weight: 800; /*字体厚重*/
    letter-spacing: 0.075em;/*    文字间隔*/
    transition: color 0.2s linear,background-color 0.2s linear;

}

.router:hover{
    cursor: pointer;
    background-color : rgba(242,242,242,0.35);
    color: rgba(255,255,255,0.6);
    /*font-size: 35px;*/
}



.waihao-index{
    position: absolute;
    font-size: 80px;
    color: white;
    font-weight: 800;
    left: 40%;
    top: 500px;
    border: 10px solid white;
}

.font{
    font-size: 30px;
    width: 100%;
    height: 100%;
    padding-top: 7.5%;
}



/*进场样式调整   */
.intro-enter,.intro-leave-to{
    top: 400px;
    opacity: 0;
}

.intro-enter-active,.intro-leave-active{
    transition: top 0.8s linear ,opacity 0.8s linear;
}

.intro-enter-to, .intro-leave{
    top: 300px;
    opacity: 1;
}

.intro-leave-active{
    transition: top 0.8s linear,opacity 0.8s linear;
}

.wh-enter,.wh-leave-to{
    top : 400px;
    opacity: 0;
}

.wh-enter-active,.wh-leave-active{
    transition: top 0.8s linear , opacity linear 0.8s;
}

.wh-enter-to,.wh-leave{
    top: 500px;
    opacity: 1;
}



@media screen and ( max-width: 500px ){
     .row-1{
         display: none;
     }

     .row-2{
         position: fixed;
         width: 100%;
         background-color: rgba(130,130,130,0.6);
         top: 0;
         z-index: 1;
         height: 5%;
         transition: 0.3s top linear;
     }
     .row-2-go{
         position: fixed;
         width: 100%;
         background-color: rgba(130,130,130,0.6);
         top: -5%;
         z-index: 1;
         height: 5%;
         transition: 0.3s top linear;
     }

     .col-1-for-router{


     }

     .router{

     }

     .font{
         font-size: 20px;
     }




}
</style>