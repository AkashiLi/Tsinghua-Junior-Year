<template>
  <el-dialog
        id="form-dialog"
        ref="myId"
        style="text-align: center"
        title="发表"
        :visible.sync="dialogVisible"
        :show-close=false
        width="80%">
    <el-form label-width="80px">  
        <el-form-item label="标题">
            <!--请修改这两行注释中间的代码来输入消息标题--> 
            <input v-model="state.title" placeholder="title"/>
            <!-- <p id="pinput">Input is {{title}} </p> -->
            <!--请修改这两行注释中间的代码来输入消息标题-->
             <span v-if="state.title_valid===false" style="color: red; padding-right: 60px;">请设置合法的title!</span>
        </el-form-item>
        <el-form-item label="内容">
            <!--请修改这两行注释中间的代码来输入消息内容-->
            <textarea v-model="state.content" placeholder="content"/>
            <!--请修改这两行注释中间的代码来输入消息内容-->
             <span v-if="state.content_valid===false" style="color: red; padding-right: 60px;">请设置合法的content!</span>
        </el-form-item>
        <el-form-item label="用户名">
            <!--请修改这两行注释中间的代码来输入用户名-->
            <input type="text" v-model="state.username" placeholder="username"
                />
            <!-- <p id="pinput">Input is {{ state.username }} </p> -->
                <!--请修改这两行注释中间的代码来输入用户名-->
        <span v-if="state.username_valid===false" style="color: red; padding-right: 60px;">请设置合法用户名!</span>
        </el-form-item>
      
    </el-form>
    <span slot="footer" class="dialog-footer">
                <!--请修改这两行注释中间的代码来产生相应按钮的点击事件-->
                <el-button v-on:click="closeDialog();">取 消</el-button>
                <el-button type="primary"
                            :disabled="state.username_valid===false || state.title_valid===false || state.content_valid===false" 
                            v-on:click="emitToParent(); closeDialog();"                
                            >确 定</el-button>
                <!--请修改这两行注释中间的代码来产生相应按钮的点击事件-->
    </span>
  </el-dialog>
</template>

<script>
export default {
  name: "PostDialog",
  props: {
    dialogVisible: {
      type: Boolean,
      default: () => true
    },
    state: {
      type: Object,
      default: () => {
          return {
          username: "",
          username_valid: false, 
          title: "",
          title_valid: false,
          content: "",
          content_valid: false,
        }
      },
    },
  },
  // 请在下方设计自己的数据结构以及事件函数
  data(){
    return {
      title: '',
      content: '',
      username: '',
      is_sent: [],
    }
  },
  created() {
    


  },
  methods: {
    // close dialog
    closeDialog() {
      this.$emit('childToParentClose', false);
      this.state.content="";
      this.state.title="";
    },
    // send information to parent
    emitToParent() {
      this.$emit('childToParent', this.state.username, this.state.title, this.state.content);
    },
  },
  watch: { // 用于实时检测username是否合法
    "state.username": {
      handler(newName) {
        this.$cookie.set("user", newName, 1);
        this.state.username_valid = /^[A-Za-z\u4e00-\u9fa5][-A-Za-z0-9\u4e00-\u9fa5_]{0,20}$/.test(newName) //username 小于等于20
      }
    },
    "state.title": {
      handler(newTitle) {
        this.state.title_valid = /^[-A-Za-z0-9 \u4e00-\u9fa5_]{1,100}$/.test(newTitle) //title can't be longer than
      }
    },
    "state.content": {
      handler(newContent) {
        this.state.content_valid = /^[-A-Za-z0-9 \u4e00-\u9fa5_]{1,500}$/.test(newContent) //content can't be longer than
      }
    }
  }
}
</script>

<style scoped>
#form-dialog {
  margin: 20px auto;
  max-width: 700px;
}

#pinput {
  margin:-10px;
}


label{
  font-size: 20px;
  display: block;
  
}
input, textarea {
  font-size: 20px;
  border: 1px double rgb(102, 97, 96) ;
  
  border-radius: 4px;
  width: 100%;
}
</style>