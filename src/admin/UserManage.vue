<template>
 <div class="articleManage">
     <h2>后台用户管理界面：</h2>
        <Table width="80%" border :columns="columns12" :data="data6">

<!--        <template slot-scope="{ row }" slot="username">-->
<!--            <strong>{{ row.username }}</strong>-->
<!--        </template>-->
<!--        <template slot-scope="{ row, index }" slot="action">-->

<!--            <Button type="primary" size="default" @click="change(index)">编辑</Button>-->
<!--            <Button type="error" size="default" @click="remove(index)">删除</Button>-->
<!--        </template>-->
            <template slot-scope="{ row }" slot="username">
                <span>{{ row.username }}</span>
            </template>

            <template slot-scope="{ row, index }" slot="name">
                <Input type="text" v-model="editName" v-if="editIndex === index" />
                <span v-else>{{ row.name }}</span>
            </template>

            <template slot-scope="{ row, index }" slot="email">
                <Input type="text" v-model="editEmail" v-if="editIndex === index" />
                <span v-else>{{row.email}}</span>
            </template>

            <template slot-scope="{ row, index }" slot="info">
                <Input type="text" v-model="editInfo" v-if="editIndex === index" />
                <span v-else>{{ row.info }}</span>
            </template>

            <template slot-scope="{ row, index }" slot="action">
                <div v-if="editIndex === index">
                    <Button @click="handleSave(index)">保存</Button>
                    <Button @click="editIndex = -1">取消</Button>
                </div>
                <div v-else>
                    <Button @click="handleEdit(row, index)">编辑</Button>

                    <Button @click="remove(index)" type="error">删除</Button>
                </div>
            </template>
        </Table>
        <Modal
        v-model="show"
        title="Title"
        :loading="loading"
        @on-ok="deleteUser">
        <p>确定要删除该用户吗？请谨慎操作。😀</p>
        </Modal>
        <Modal
             v-model="change"
             title="Title"
             :loading="loading"
             @on-ok="changeUser">
         <p>请再次确定要更改的用户信息。😀</p>
        </Modal>
 </div>
</template>
<script>
import { getnotedetail, PostMessage } from '../components/NetWork/request'
    export default {
        mounted() {
            this.getUserInfo()
        },
        data () {
            return {
                editIndex: -1,  // 当前聚焦的输入框的行数
                editUsername: '111',  // 第一列输入框，当然聚焦的输入框的输入内容，与 data 分离避免重构的闪烁
                editName: '222',  // 第二列输入框
                editEmail: '',  // 第三列输入框
                editInfo: '',  // 第四列输入框
                show: false,
                change:false,
                loading: true,
                index:0,
                columns12: [
                    {
                        title: '用户名',
                        slot: 'username',
                        width: 200,
                    },
                    {
                        title: '昵称',
                        slot: 'name',
                        width: 200,
                    },
                    {
                        title: '邮箱',
                        slot: 'email',
                        width: 200,
                    },
                    {
                        title: '简介',
                        slot: 'info',
                        width: 250,
                    },
                    {
                        title: 'Action',
                        slot: 'action',
                        width: 384,
                        align: 'center'
                    }
                ],
                data6: []
            }
        },
        methods: {
            getUserInfo() {
                getnotedetail('/user/getUserInfo')
                .then(res => {
                    if(res.data.err === 0) {
                        this.data6 = res.data.message
                        this.$Message.success('数据拉取成功!')
                    } else {
                        this.$Message.error('数据拉取失败!')
                    }
                })
            },
            handleEdit(row,index){
                this.editUsername=row.username;
                this.editName=row.name;
                this.editEmail=row.email;
                this.editInfo=row.info;
                this.editIndex=index;

            },
            handleSave (index) {
                this.data6[index].username = this.editUsername;
                this.data6[index].name = this.editName;
                this.data6[index].email = this.editEmail;
                this.data6[index].info = this.editInfo;
                this.editIndex = -1;
                this.change=true;
            },
            remove (index) {
                this.show = true
                this.index = index
            },
            changeUser(){
                PostMessage('/user/changeUserInfo',{username:this.editUsername,name:this.editName,email:this.editEmail,info:this.editInfo})
                .then(res => {
                    if(res.data.err == 0) {
                        this.$Message.success(res.data.message)
                        this.change = false
                    }else {
                        this.$Message.error(res.data.message)
                    }
                }).catch(reason => {
                    this.$Message.error(reason.message)
                })
            },
            deleteUser() {
                PostMessage('/user/deleteUserInfo', {username:this.data6[this.index].username})
                .then(res => {
                    if(res.data.err == 0) {
                        this.$Message.success(res.data.message)
                        this.show = false
                    }else {
                        this.$Message.error(res.data.message)
                    }
                }).catch(reason => {
                     this.$Message.error(reason.message)
                })
            }
        }
    }
</script>
<style lang="scss" scoped>
.articleManage {
    padding-top: 5rem;
    padding-right: 15rem;
    position: relative;
    z-index: 99;
    h2 {
        margin-bottom: 1rem;
    }
}
</style>