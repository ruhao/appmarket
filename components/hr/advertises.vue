<template>
	<div class="content">
		<Input v-model="fliter.title">
		<Button slot="append" icon="ios-search" @click="search"></Button>
		<Button type="success" slot="append" style="width: 80px;margin-left: 10px;background: lightgreen;color: white;" @click="onAdd">添加信息</Button>
		</Input>
		
		<div class="content-body">
			<Table border :columns="columns7" :data="fliter.data6"></Table>
		</div>
		<div class="content-foot">
			<Page :total="fliter.total" show-elevator @on-change="changePage"></Page>
		</div>
		<Modal v-model="modal6" title="留言详情" :loading="loading" @on-ok="asyncOK">
			<Form ref="formValidate" :model="formValidate" :rules="ruleValidate" :label-width="80">
				<FormItem label="人才类型" prop="title">
					<Input v-model="formValidate.title" placeholder="Enter your title"></Input>
				</FormItem>
				<FormItem label="数量" prop="title">
					<Input v-model="formValidate.num" placeholder="Enter your num"></Input>
				</FormItem>
				<FormItem label="学历要求" prop="title">
					<Input v-model="formValidate.education" placeholder="Enter your education"></Input>
				</FormItem>
				<FormItem label="年龄要求" prop="title">
					<Input v-model="formValidate.age" placeholder="Enter your age"></Input>
				</FormItem>
				<FormItem label="薪资" prop="title">
					<Input v-model="formValidate.pay" placeholder="Enter your pay"></Input>
				</FormItem>
				<FormItem label="语言要求" prop="title">
					<Input v-model="formValidate.language" placeholder="Enter your language"></Input>
				</FormItem>
				<FormItem label="性别" prop="title">
					<Input v-model="formValidate.sex" placeholder="Enter your sex"></Input>
				</FormItem>
				<FormItem label="联系电话" prop="telephone">
					<Input v-model="formValidate.telephone" placeholder="Enter your telephone"></Input>
				</FormItem>
				<FormItem label="工作地址" prop="city">
					<Select v-model="formValidate.site" placeholder="Select your site">
						<Option value="杭州">杭州</Option>
					</Select>
				</FormItem>
				<FormItem label="详细要求" prop="detail">
					<Input v-model="formValidate.detail" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Enter something..."></Input>
				</FormItem>
				<FormItem>
					<div v-if="formValidate.type"><!--通过type是否存在来判断该按钮是新增还是修改，触发的事件也不相同-->
					<Button type="primary" @click="handleUpdate('formValidate')">修改</Button>
					</div>
					<div v-else>
					<Button type="primary" @click="handleSubmit1('formValidate')">新增</Button>
					</div>
				</FormItem>
			</Form>
		</Modal>
	</div>
</template>
<script>
import Common from "../common/hm.js";
export default {
  mixins: [Common], //代码混入，通过公用Js代码混入来减少代码量
  data() {
    return {
      apimodel: "hr", //由于代码混入，通过restfulapi来改变一个变量达到访问不同的数据库的目的
      type: "3", //分类type，每个库都会有多个不同切换，每次切换通过改变不同的type来达到数据改变，从而来再次渲染视图层
      cateId: "5a6af8171484f90a8854f178", //给上父元素的ID方便分类
      columns7: [
        {
          //后台管理页面表格建立
          title: "职位",
          key: "title"
        },
        {
          title: "数量",
          key: "num"
        },
        {
          title: "工作地址",
          key: "site"
        },
        {
          title: "联系人电话",
          key: "telephone"
        },
        {
          title: "发布时间",
          key: "date"
        },
        {
          title: "操作",
          key: "action",
          width: 150,
          align: "center",
          render: (h, params) => {
            return h("div", [
              h(
                "Button",
                {
                  props: {
                    type: "primary",
                    size: "small"
                  },
                  style: {
                    marginRight: "5px"
                  },
                  on: {
                    click: () => {
                      this.show(params.index);
                    }
                  }
                },
                "查看"
              ),
              h(
                "Button",
                {
                  props: {
                    type: "error",
                    size: "small"
                  },
                  on: {
                    click: () => {
                      this.remove(params.index);
                    }
                  }
                },
                "删除"
              )
            ]);
          }
        }
      ],
      fliter: {
        //渲染的内容
        data6: [],
        total: 0,
        limit: 12,
        page: 1,
        type: "3",
        title: ""
      },
      formValidate: {
        //表格的数据绑定。进行大部分数据库交互
        title: "",
        num: "",
        site: "",
        education: "",
        detail: "",
        date: "",
        age: "",
        pay: "",
        language: "",
        telephone: "",
        sex: "",
        type: ""
      },
      ruleValidate: {
        //表单验证
        title: [
          {
            required: true,
            message: "The name cannot be empty",
            trigger: "blur"
          }
        ],
        site: [
          {
            required: true,
            message: "Please select the city",
            trigger: "change"
          }
        ],
        detail: [
          {
            required: true,
            message: "Please enter a personal detail",
            trigger: "blur"
          },
          {
            type: "string",
            min: 15,
            message: "Introduce no less than 15 words",
            trigger: "blur"
          }
        ],
        telephone: [
          {
            required: true,
            message: "Please enter a personal telephone",
            trigger: "blur"
          },
          {
            validator: this.validateMobile,
            trigger: "blur"
          }
        ]
      }
    };
  },
  methods: {
    validateMobile(rule, value, callback) {
      //自定义正则表单验证
      let reg = /^1[3|5|7|8]\d{9}$/;
      if (reg.test(value)) {
        callback(); //执行回调函数
      } else {
        callback(new Error("Please enter the correct phone number"));
      }
    },
    handleUpdate() {
      this.formValidate.date = new Date();
      this.$http
        .put(
          "http://120.79.22.222:3000/hr/data/" + this.formValidate._id,
          this.formValidate
        )
        .then(res => {
          //改变数据，通过this.formValidate._id找到相应的数据，然后把this.formValidate导入进行不一样的修改
          this.getData();
          this.modal6 = false;
        });
    },
    handleSubmit1() {
      this.formValidate.type = this.type;
      this.formValidate.cateId = this.cateId;
      this.formValidate.date = new Date();
      console.log(this.formValidate);
      //把cateID,type分类，时间更新等附加上去
      this.$http
        .post(
          "http://120.79.22.222:3000/hr/data",
          this.formValidate
        )
        .then(res => {
          console.log(res);
          this.getData();
          this.modal6 = false;
          this.formValidate.type = "";
        });
    }
  },
  created() {
    //生命周期
    this.cateid = this.$route.params.id; //获取传递的id
    this.getData(); //获取数据，为后续的渲染做准备
  }
};
</script>
<style scoped>
.content {
  height: 100%;
  display: flex;
  flex-direction: column;
}

.content-body {
  flex: 1;
  margin-top: 20px;
}

.content-foot {
  height: 50px;
  text-align: center;
}

.message-date {
  min-height: 550px;
  display: flex;
  flex-direction: column;
  padding-bottom: 50px;
}

.message-date div:nth-of-type(1) {
  text-align: center;
  margin-top: 20px;
  height: 30px;
  font-size: 22px;
  font-weight: 600;
  text-shadow: 1px 1px 1px #999999;
}

.message-date div:nth-of-type(2) {
  text-align: left;
  text-indent: 2em;
  flex: 1;
  margin-top: 30px;
  color: black;
}

.bottom {
  width: 250px;
  margin-left: 300px;
  height: 20px;
  text-align: left;
  margin-top: 5px;
}
</style>