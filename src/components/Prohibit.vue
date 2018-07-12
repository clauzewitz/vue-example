<template>
    <div>
        <Alert ref="alert"/>
        <b-card title="금칙어 관리"
            sub-title="금칙어 조회하기">
            <div class="row mb-3 justify-content-end">
                <b-input-group class="col-6 pr-0">
                    <b-form-input v-model="searchVal"></b-form-input>
                    <b-input-group-append>
                        <b-btn variant="primary" @click="searchProhibitWord">검색</b-btn>
                    </b-input-group-append>
                </b-input-group>
            </div>
            <div class="row mb-3 justify-content-end">
                <b-btn variant="primary" @click="showAddProhibitWord">등록</b-btn>
            </div>
            <div class="row">
                <table class="col">
                    <colgroup>
                        <col width="90%"/>
                        <col width="10%"/>
                    </colgroup>
                    <thead>
                        <tr class="text-center">
                            <th>금칙어</th>
                            <th>삭제</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-if="!hasProhibitWord">
                            <td class="text-center" colspan="2">내역이 존재하지 않습니다</td>
                        </tr>
                        <tr v-else v-for="prohibitWord in prohibitWords" :key="prohibitWord.word">
                            <td>{{prohibitWord.word}}</td>
                            <td>
                                <b-btn class="w-100" variant="warning" @click="showDeleteProhibitWord(prohibitWord.idx)">삭제</b-btn>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </b-card>
        <ConfirmModal ref="confirmModal" v-on:handleOk="deleteProhibitWord"/>
        <PromptModal ref="promptModal" v-on:handleOk="addProhibitWord"/>
    </div>
</template>

<script>
    import Resource from './Resource'

    import Alert from './Alert'
    import ConfirmModal from './ConfirmModal'
    import PromptModal from './PromptModal'

    export default {
        name: 'Prohibit',
        components: {
            Alert,
            ConfirmModal,
            PromptModal
        },
        data () {
            return {
                searchVal: undefined,
                prohibitWords: []
            }
        },
        computed: {
            hasProhibitWord: function () {
                return this.prohibitWords && this.prohibitWords.length > 0
            }
        },
        methods: {
            searchProhibitWord () {
                Resource.get('/list', {
                        searchVal: this.searchVal
                }).then(
                    result => {
                        if (result.data.status == 1) {
                            this.prohibitWords = result.data.result.list
                        }
                    },
                    error => {
                        this.$refs.alert.show('검색에 실패하였습니다', Alert.TYPE.DANGER)
                    }
                )
            },
            showAddProhibitWord () {
                this.$refs.promptModal.show('등록', '영단어는 소문자로 등록됩니다')
            },
            showDeleteProhibitWord (index) {
                this.$refs.confirmModal.show('삭제', '삭제하시겠습니까?', index)
            },
            addProhibitWord (word) {
                Resource.post('/add.json', {
                    word: word
                }).then(
                    result => {
                        this.$refs.alert.show('등록되었습니다')
                    },
                    error => {
                        this.$refs.alert.show('등록에 실패하였습니다', Alert.TYPE.DANGER)
                    }
                )
            },
            deleteProhibitWord (index) {
                Resource.post('/del.json', {
                    idx: index
                }).then(
                    result => {
                        this.$refs.alert.show('삭제되었습니다')
                        this.searchProhibitWord()
                    },
                    error => {
                        this.$refs.alert.show('삭제에 실패하였습니다', Alert.TYPE.DANGER)
                    }
                )
            }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    h1, h2 {
        font-weight: normal;
    }
    ul {
        list-style-type: none;
        padding: 0;
    }
    li {
        display: inline-block;
        margin: 0 10px;
    }
    a {
        color: #42b983;
    }
</style>
