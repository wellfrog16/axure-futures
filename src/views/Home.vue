<template>
    <el-container class="app-container">
        <el-header>科技监管平台</el-header>
        <el-container>
            <el-aside width="auto">
                <!-- <div class="switch"><span @click="handleCollapse">关</span></div> -->
                <el-menu router :collapse="isCollapse">
                    <template v-for="(item, index) in $router.options.routes">

                        <!-- 无二级子节点 -->
                        <el-menu-item v-if="!item.children && !item.hidden" :index="item.path" :key="index">
                            <i :class="item.icon"></i>
                            <span slot="title">{{ item.name }}</span>
                        </el-menu-item>

                        <!-- 有二级子节点 -->
                        <el-submenu v-if="item.children && !item.hidden" :index="item.name" :key="index">
                            <template slot="title"><i :class="item.icon"></i><span>{{ item.name }}</span></template>                            
                            <template v-for="(item1, index1) in item.children">
                                <!-- 无三级菜单 -->
                                <el-menu-item v-if="!item1.children" :index="`${item.path}/${item1.path}`" :key="index1"><i :class="item1.icon"></i>{{ item1.name }}</el-menu-item>

                                <!-- 有三级菜单 -->
                                <template v-if="item1.children">
                                    <el-submenu :index="item1.name" :key="index1">
                                        <template slot="title"><i :class="item1.icon"></i><span>{{ item1.name }}</span></template>
                                        <el-menu-item
                                            v-for="(item2, index2) in item1.children"
                                            :index="`${item.path}/${item1.path}/${item2.path}`"
                                            :key="index2">
                                            <i :class="item2.icon"></i>{{ item2.name }}
                                        </el-menu-item>
                                    </el-submenu>
                                </template>
                            </template>
                        </el-submenu>
                        <!--  -->
                    </template>
                </el-menu>
            </el-aside>
            <el-main>
                <el-col :span="24" class="breadcrumb">
                    <el-breadcrumb separator="/" class="breadcrumb-inner">
                        <el-breadcrumb-item v-for="item in $route.matched" :key="item.path">{{ item.name }}</el-breadcrumb-item>
                    </el-breadcrumb>
                    <strong class="title">{{$route.name}}</strong>
                </el-col>
                <transition name="fade" mode="out-in">
                    <router-view/>
                </transition>
            </el-main>
        </el-container>
    </el-container>
</template>

<script>
import $ from 'jquery';
    export default {
        data() {
            return {
                isCollapse: true
            }
        },
        methods: {
            handleCollapse() {
                this.isCollapse = !this.isCollapse;
                $('.switch span').text(this.isCollapse ? '开': '关');
            }
        }
    };
</script>

<style lang="less">
@import '../assets/style/config.less';

.app-container {
    flex-direction: column;
    height: 100vh;

    >header {
        flex: 0 0 60px;
        line-height: 60px;
        background-color: @color-primary;
        color:#fff;
    }

    >section {

        >aside {
            // flex: 0 0 200px;

            >ul {
                height: 100%;
            }
        }
    }

    .switch {
        text-align: right;
        height: 20px;
        line-height: 20px;
        font-size: 14px;
        padding: 10px 25px;
        cursor: pointer;
    }
}

.breadcrumb {
    display: flex;
    justify-content: space-between;
    margin-bottom: 10px;
}
</style>