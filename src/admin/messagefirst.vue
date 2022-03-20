<template>
    <div class="articleManage">
        <h2>一级消息管理：</h2>
        <filter-table @on-search="onSearch"
                      :data="data6"
                      :columns="columns12">
            <template slot-scope="{ row, index }" slot="action">
                <div>
                    <Button @click="remove(index)" type="error">删除</Button>
                </div>
            </template>
        </filter-table>
<!--        <Table width="100%" border :columns="columns12" :data="data6">-->
<!--            <template slot-scope="{ row }" slot="username">-->
<!--                <span>{{ row.username }}</span>-->
<!--            </template>-->
<!--            <template slot-scope="{ row }" slot="id">-->
<!--                <span>{{ row.id }}</span>-->
<!--            </template>-->
<!--            <template slot-scope="{ row }" slot="name">-->
<!--                <span>{{ row.name }}</span>-->
<!--            </template>-->

<!--            <template slot-scope="{ row }" slot="value">-->
<!--                <span>{{row.value}}</span>-->
<!--            </template>-->

<!--            <template slot-scope="{ row}" slot="date">-->
<!--                <span>{{ row.date}}</span>-->
<!--            </template>-->

<!--            <template slot-scope="{ row, index }" slot="action">-->
<!--                <div>-->
<!--                    <Button @click="check(index)">查看回复</Button>-->
<!--                    <Button @click="remove(index)" type="error">删除</Button>-->
<!--                </div>-->
<!--            </template>-->
<!--        </Table>-->
        <Page class="page" :page-size="10" @on-change="Pagechange" :total="count" show-total />
        <Modal
                v-model="show"
                title="Title"
                :loading="loading"
                @on-ok="deleteMessage">
            <p>确定要删除该评论吗？请谨慎操作。😀</p>
        </Modal>
    </div>
</template>
<script>
    import { PageSizeChange } from '../components/NetWork/request'
    import {  PostMessage } from '../components/NetWork/request'
    import FilterTable from '../components/filtertable/FilterTable';
    export default {
        name: "messagefirst",
        components: {FilterTable},
        mounted(){
            this.Pagechange(1)
        },
        data () {
            return {
                isshow:false,
                checkIndex: -1,  // 当前聚焦的输入框的行数
                show: false,
                loading: true,
                index:0,
                columns12: [
                    {
                        title: '用户名',
                        key: 'username',
                        width: 200,
                        align: 'center',
                        filter: {
                            type: 'Input'
                        }
                    },
                    {
                        title: 'id',
                        key: 'id',
                        width:100,
                        align: 'center',
                        filter: {
                            type: 'Input'
                        }
                    },
                    {
                        title: '昵称',
                        key: 'name',
                        width: 200,
                        align: 'center',
                        filter: {
                            type: 'Input'
                        }
                    },
                    {
                        title: '留言',
                        key: 'value',
                        width: 384,
                        align: 'center',
                        filter: {
                            type: 'Input'
                        }
                    },
                    {
                        title: '日期',
                        key: 'date',
                        width: 150,
                        align: 'center',
                        filter: {
                            type: 'Input'
                        }
                    },
                    {
                        title: 'Action',
                        key: 'action',
                        width: 200,
                        align: 'center',
                        filter: {
                            type: 'Input'
                        },
                        render: (h, params) => {
                            return h('div', [
                                h('Button', {
                                    props: { type: 'primary', size: 'small' },
                                    on: {
                                        click: () => { this.check(params.index); }
                                    }
                                }, '查看回复'),
                                h('Button', {
                                    props: { type: 'error', size: 'small' },
                                    on: {
                                        click: () => { this.remove(params.index); }
                                    }
                                }, '删除')

                            ]);
                        }
                    }
                ],
                count:0,
                data6: []
            }
        },
        methods: {
            onSearch(search) {
                PostMessage("/page/serchmessage", { value: search }).then(
                    res => {
                        if (res.data.err === 0) {
                            let count = res.data.message.length;
                            this.data6 = res.data.message;
                            if (count === 0) {
                                this.$Message.success("无相关数据");
                            } else {
                                this.$Message.success("查询成功了!");
                            }
                        } else {
                            this.$Message.error(res.data.message);
                        }
                    }
                );
                // alert('查询条件：'+JSON.stringify(search,null,4));
                // console.log(search)
            },
            Pagechange(index) {
                /* 发起请求 */
                this.$store.commit('LoadingTitleChange', {isshow: true, title: '正在获取留言信息~'})
                PageSizeChange('/page/pageSize',{page:index})
                    .then(res => {
                        if(res.data.err == 0) {
                            this.count = res.data.message.count
                            this.data6 = res.data.message.data
                            this.$Spin.hide()
                            this.$Message.success('数据拉取成功!')
                        } else {
                            this.$Message.error('数据拉取失败!')
                        }
                        this.$store.commit('LoadingTitleChange', {isshow: false, title: ''})

                    })
            },
            remove (index) {
                this.show = true
                this.index = index
            },
            check(index){
                this.checkIndex = index
                this.$router.push({path:'/admin/article/upload/messagesecond/',query: {reply: this.data6[index].replyAccess}})
            },
            deleteMessage() {
                PostMessage('/page/deleteMessage', {id:this.data6[this.index].id})
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