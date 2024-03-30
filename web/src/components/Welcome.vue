<template>
  <div class="welcome">
    <div class="container">
      <h1 class="title">我今天能帮你做什么？</h1>

      <el-row :gutter="20">
        <el-col :span="8">
          <div class="grid-content">
            <div class="list-box">
              <ul>
                <li v-for="item in samples" :key="item"><a @click="send(item)">{{ item }}</a></li>
              </ul>
            </div>
          </div>
        </el-col>
        <el-col :span="8">
          <div class="grid-content">
            <div class="list-box">
              <ul>
                <li v-for="item in plugins" :key="item"><a @click="send(item)">{{ item }}</a></li>
              </ul>
            </div>
          </div>
        </el-col>
        <el-col :span="8">
          <div class="grid-content">
            <div class="list-box">
              <ul>
                <li v-for="item in capabilities" :key="item">
                  <span v-if="item.value === ''">{{ item.text }}</span>
                  <a @click="send(item.value)" v-else>{{ item.text }}</a>
                </li>
              </ul>
            </div>
          </div>
        </el-col>
      </el-row>
    </div>
  </div>
</template>
<script setup>

import {onMounted, ref} from "vue";
import {httpGet} from "@/utils/http";
import {ElMessage} from "element-plus";

const title = ref(process.env.VUE_APP_TITLE)

const samples = ref([
  "用小学生都能听懂的术语解释什么是量子纠缠",
  "能给一位6岁男孩的生日会提供一些创造性的建议吗？",
  "如何用 Go 语言实现支持代理 Http client 请求?"
])

const plugins = ref([
  "我想知道人工智能在日常生活中有哪些应用？",
  "你好，我是第一次使用这个聊天机器人，感觉有点陌生。",
  "我不太了解人工智能，它是怎么工作的？"
])

const capabilities = ref([
  {
    text: "轻松扮演翻译专家，程序员，AI 女友，文案高手...",
    value: ""
  },
  {
    text: "国产大语言模型支持，百度文心，科大讯飞，ChatGLM...",
    value: ""
  },
  {
    text: "绘画：马斯克开拖拉机，20世纪，中国农村。3:2",
    value: "绘画：马斯克开拖拉机，20世纪，中国农村。3:2"
  }
])

onMounted(() => {
  httpGet("/api/config/get?key=system").then(res => {
    title.value = res.data.title
  }).catch(e => {
    ElMessage.error("获取系统配置失败：" + e.message)
  })
})

const emits = defineEmits(['send']);
const send = (text) => {
  emits('send', text)
}
</script>
<style scoped lang="stylus">
.welcome {
  text-align center
  display flex
  justify-content center
  margin-top 8vh

  .container {
    max-width 768px;
    width 100%

    .title {
      font-size: 2.25rem
      line-height: 2.5rem
      font-weight 600
      margin-bottom: 4rem
    }

    .grid-content {
      .item-title {
        div {
          padding 6px 10px;

          .iconfont {
            font-size 24px;
          }
        }
      }

      .list-box {
        ul {
          padding 10px;

          li {
            font-size 14px;
            padding .75rem
            border-radius 5px;
            background-color: rgba(247, 247, 248, 1);

            line-height 1.5
            color #666666

            a {
              cursor pointer
              display block
              width 100%
            }
            margin-top 10px;
          }
        }
      }
    }
  }
}
</style>