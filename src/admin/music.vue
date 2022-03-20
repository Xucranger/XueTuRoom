<template>
    <div class="articleManage">
        <h2>音乐管理：</h2>
        <Input
                v-model="content"
                maxlength="200"
                show-word-limit
                type="textarea"
                placeholder="输入图片描述..."
                style="width: 500px;"
        />
        <Button @click.native="sendMusic" type="primary">添加音乐</Button>
        <Table  width="auto" :columns="columns12" :data="data6">

        </Table>
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
    import {getnotedetail, PostMessage} from '../components/NetWork/request'
    export default {
        name: "music",

        mounted(){
            this.getmusic()
        },
        data () {
            return {
                content:" ",
                isshow:false,
                checkIndex: -1,  // 当前聚焦的输入框的行数
                show: false,
                loading: true,
                index:0,
                columns12: [
                    {
                        title: 'musicid',
                        key: 'music_id',
                        width: 200,
                        align: 'center'

                    },
                    {
                        title: 'id',
                        key: 'id',
                        width:100,
                        align: 'center'

                    },
                ],
                count:0,
                data6: []
            }
        },
        methods: {
            sendMusic() {
                if (this.content) {
                    PostMessage("/upload/Music", {
                        contents: this.content,
                    }).then((res) => {
                        if (res.data.err === 0) {
                            this.$Message.success(res.data.data);
                        } else {
                            this.$Message.error(res.data.data);
                        }
                    });
                } else {
                    this.$Message.error("内容不能为空!");
                }
            },
            getmusic()
            {
                getnotedetail('/page/getmusic')
                .then(res=>{
                    if(res.data.err === 0) {
                        this.data6 = res.data.message
                        this.$Message.success('数据拉取成功!')
                    } else {
                        this.$Message.error('数据拉取失败!')
                    }
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