<template>
    <div class="articleManage">
        <h2>二级消息管理：</h2>
        <filter-table @on-search="onSearch"
                      :data="data6"
                      :columns="columns12">
        </filter-table>
        <Modal
                v-model="show"
                title="Title"
                :loading="loading"
                @on-ok="deletetwoMessage">
            <p>确定要删除该评论吗？请谨慎操作。😀</p>
        </Modal>
    </div>
</template>
<script>
    import {  PostMessage} from '../components/NetWork/request';
    import FilterTable from '../components/filtertable/FilterTable';
    export default {
        name: "messagesecond",
        components: {FilterTable},
        created() {
            this.data6 = this.$route.query.reply;
        },
        data () {
            return {
                isshow:false,
                show: false,
                loading: true,
                index:0,
                columns12: [
                    {
                        title: '用户名',
                        key:'username',
                        width: 200,
                        align: 'center',
                        filter: {
                            type: 'Input'
                        }
                    },
                    {
                        title: 'id',
                        key: 'id',
                        width:150,
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
                        key: 'content',
                        width: 384,
                        align: 'center',
                        filter: {
                            type: 'Input'
                        }
                    },
                    {
                        title: '日期',
                        key: 'datetime',
                        width: 150,
                        align: 'center',
                        filter: {
                            type: 'Input'
                        }
                    },
                    {
                        title: '操作',
                        slot: 'action',
                        width: 150,
                        align: 'center',
                        render: (h, params) => {
                            return h('div', [
                                h('Button', {
                                    props: { type: 'error', size: 'small' },
                                    on: {
                                        click: () => { this.remove(params.index); }
                                    }
                                }, '删除')
                            ]);
                        }
                    },

                ],
                count:0,
                data6: []
            }
        },
        methods: {
            onSearch(search) {
                PostMessage("/page/serchtwomessage", { value: search }).then(
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
            remove (index) {
                this.show = true
                this.index = index
            },
            deletetwoMessage() {
                PostMessage('/page/deletetwoMessage', {id:this.data6[this.index].id})
                    .then(res => {
                        if(res.data.err == 0) {
                            this.$Message.success(res.data.message)
                            this.show = false
                            this.$router.back()
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