# elment-ui-vue.js-tree
elment-ui配合vue.js写的dom树  (增  删  改) 



               <link rel="stylesheet" href="https://unpkg.com/element-ui/lib/theme-chalk/index.css">
              <script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
              <script src="https://unpkg.com/element-ui/lib/index.js"></script>

                    <style>
                        .expand {
                            width: 100%;
                            height: 100%;
                            overflow: hidden;
                        }

                        .expand>div {
                            height: 85%;
                            padding-top: 20px;
                            width: 50%;
                            margin: 20px auto;
                            max-width: 400px;
                            overflow-y: auto;
                        }

                        .expand>div::-webkit-scrollbar-track {
                            box-shadow: inset 0 0 6px rgba(0, 0, 0, .3);
                            border-radius: 5px;
                        }

                        .expand>div::-webkit-scrollbar-thumb {
                            background-color: rgba(50, 65, 87, 0.5);
                            outline: 1px solid slategrey;
                            border-radius: 5px;
                        }

                        .expand>div::-webkit-scrollbar {
                            width: 10px;
                        }

                        .expand-tree {
                            border: none;
                            margin-top: 10px;
                        }

                        .expand-tree .el-tree-node.is-current,
                        .expand-tree .el-tree-node:hover {
                            overflow: hidden;
                        }

                        .expand-tree .is-current>.el-tree-node__content .tree-btn,
                        .expand-tree .el-tree-node__content:hover .tree-btn {
                            display: inline-block;
                        }

                        .expand-tree .is-current>.el-tree-node__content .tree-label {
                            font-weight: 600;
                            white-space: normal;
                        }

                        .tree-expand {
                            overflow: hidden;
                        }

                        .tree-expand .tree-label.tree-new {
                            font-weight: 600;
                        }

                        .tree-expand .tree-label {
                            font-size: 0.9em;
                        }

                        .tree-expand .tree-label .edit {
                            width: 80%;
                        }

                        .tree-expand .tree-btn {
                            display: none;
                            float: right;
                            margin-right: 20px;
                        }

                        .tree-expand .tree-btn i {
                            color: #8492a6;
                            font-size: 0.9em;
                            margin-right: 3px;
                        }
                    </style>
                    
                    <div id="app">
                      <template>
                          <div class="expand">
                              <div>
                                  <el-button @click="handleAddTop">添加顶级节点</el-button>
                                  <el-tree ref="expandMenuList" class="expand-tree"
                                           v-if="isLoadingTree"
                                           :data="setTree"
                                           node-key="id"
                                           highlight-current
                                           :props="defaultProps"
                                           :expand-on-click-node="false"
                                           :render-content="renderContent"
                                           :default-expanded-keys="defaultExpandKeys"
                                           @node-click="handleNodeClick">
                                  </el-tree>
                              </div>
                          </div>

                      </template>
                  </div>

                    
                <script>
                    var TreeRender = {
                        template: `<span class="tree-expand">
                            <span class="tree-label" v-show="DATA.isEdit">
                            <el-input class="edit" size="mini" autofocus v-model="DATA.name" :ref="'treeInput'+DATA.id"
                            @click.stop.native="nodeEditFocus"
                            @blur.stop="nodeEditPass(STORE,DATA,NODE)"
                            @keyup.enter.stop.native="nodeEditPass(STORE,DATA,NODE)"></el-input>
                            </span>
                            <span v-show="!DATA.isEdit" :class="[DATA.id > maxexpandId ? 'tree-new tree-label' : 'tree-label']">
                            <span>{{DATA.name}}</span>
                        </span>
                        <span class="tree-btn" v-show="!DATA.isEdit">
                            <i class="el-icon-plus" @click.stop="nodeAdd(STORE,DATA,NODE)"></i>
                            <i class="el-icon-edit" @click.stop="nodeEdit(STORE,DATA,NODE)"></i>
                            <i class="el-icon-delete" @click.stop="nodeDel(STORE,DATA,NODE)"></i>
                            </span>
                            </span>`,
                           props: ['NODE', 'DATA', 'STORE', 'maxexpandId'],
                            methods: {
                          nodeAdd(s, d, n) { //新增
                            this.$emit('nodeAdd', s, d, n)
                          },
                          nodeEdit(s, d, n) { //编辑
                            d.isEdit = true;
                            this.$nextTick(() => {
                                this.$refs['treeInput' + d.id].$refs.input.focus()
                         })
                          this.$emit('nodeEdit', s, d, n);
                         },
                         nodeDel(s, d, n) { //删除
                         this.$emit('nodeDel', s, d, n)
                         },
                        nodeEditPass(s, d, n) { //编辑完成
                        d.isEdit = false;
                        },
                       nodeEditFocus() {
                        //阻止点击节点的事件冒泡
                        },
                      }
                      }
                    let api={
                        maxexpandId: 95,
                        treelist: [{
                            id: 1,
                            name: "北京市",
                            ProSort: 1,
                            remark: "直辖市",
                            pid: '',
                            isEdit: false,
                            children: [{
                                id: 35,
                                name: "朝阳区",
                                pid: 1,
                                remark: '',
                                isEdit: false,
                                children: []
                            }, {
                                id: 36,
                                name: "海淀区",
                                pid: 1,
                                remark: '',
                                isEdit: false,
                                children: []
                            }, {
                                id: 37,
                                name: "房山区",
                                pid: 1,
                                remark: '',
                                isEdit: false,
                                children: []
                            }, {
                                id: 38,
                                name: "丰台区",
                                pid: 1,
                                remark: '',
                                isEdit: false,
                                children: []
                            }, {
                                id: 39,
                                name: "昌平区",
                                pid: 1,
                                remark: '',
                                isEdit: false,
                                children: []
                            }, {
                                id: 40,
                                name: "大兴区",
                                pid: 1,
                                remark: '',
                                isEdit: false,
                                children: []
                            }]
                        }, {
                            id: 2,
                            name: "天津市",
                            ProSort: 2,
                            remark: "直辖市",
                            pid: '',
                            isEdit: false,
                            children: [{
                                id: 41,
                                name: "北辰区",
                                pid: 2,
                                remark: '',
                                isEdit: false,
                                children: []
                            }, {
                                id: 42,
                                name: "河北区",
                                pid: 2,
                                remark: '',
                                isEdit: false,
                                children: []
                            }, {
                                id: 43,
                                name: "西青区",
                                pid: 2,
                                remark: '',
                                isEdit: false,
                                children: []
                            }, {
                                id: 44,
                                name: "东丽区",
                                pid: 2,
                                remark: '',
                                isEdit: false,
                                children: []
                            }]
                        }, {
                            id: 3,
                            name: "河北省",
                            ProSort: 5,
                            remark: "省份",
                            pid: '',
                            isEdit: false,
                            children: [{
                                id: 45,
                                name: "衡水市",
                                pid: 3,
                                remark: '',
                                isEdit: false,
                                children: []
                            }, {
                                id: 46,
                                name: "张家口市",
                                pid: 3,
                                remark: '',
                                isEdit: false,
                                children: []
                            }, {
                                id: 47,
                                name: "秦皇岛市",
                                pid: 3,
                                remark: '',
                                isEdit: false,
                                children: []
                            }, {
                                id: 48,
                                name: "承德市",
                                pid: 3,
                                remark: '',
                                isEdit: false,
                                children: []
                            }, {
                                id: 49,
                                name: "邯郸市",
                                pid: 3,
                                remark: '',
                                isEdit: false,
                                children: []
                            }]
                        }, {
                            id: 4,
                            name: "山西省",
                            ProSort: 6,
                            remark: "省份",
                            pid: '',
                            isEdit: false,
                            children: [{
                                id: 50,
                                name: "大同市",
                                pid: 4,
                                remark: '',
                                isEdit: false,
                                children: []
                            }]
                        }]
                    };
                    var vm = new Vue({
                        el: "#app",
                        data: {
                            maxexpandId: api.maxexpandId, //新增节点开始id
                            non_maxexpandId: api.maxexpandId, //新增节点开始id(不更改)
                            isLoadingTree: false, //是否加载节点树
                            setTree: api.treelist, //节点树数据
                            defaultProps: {
                                children: 'children',
                                label: 'name'
                            },
                            defaultExpandKeys: [], //默认展开节点列表
                        },
                        mounted() {
                        console.log(api)
                        this.initExpand()
                    },
                    methods: {
                        initExpand() {
                            this.setTree.map((a) => {
                                this.defaultExpandKeys.push(a.id)
                        });
                        this.isLoadingTree = true;
                    },
                    handleNodeClick(d, n, s) { //点击节点
                        // console.log(d,n)
                        d.isEdit = false; //放弃编辑状态
                    },
                    renderContent(h, {
                        node,
                        data,
                        store
                     }) { //加载节点
                        let that = this;
                        return h(TreeRender, {
                            props: {
                                DATA: data,
                                NODE: node,
                                STORE: store,
                                maxexpandId: that.non_maxexpandId
                            },
                            on: {
                                nodeAdd: ((s, d, n) => that.handleAdd(s, d, n)),
                                nodeEdit: ((s, d, n) => that.handleEdit(s, d, n)),
                                nodeDel: ((s, d, n) => that.handleDelete(s, d, n))
                           }
                         });
                      },
                    handleAddTop() {
                        this.setTree.push({
                            id: ++this.maxexpandId,
                            name: '新增节点',
                            pid: '',
                            isEdit: false,
                            children: []
                        })
                    },
                    handleAdd(s, d, n) { //增加节点
                        console.log(s, d, n)
                        if (n.level >= 6) {
                            this.$message.error("最多只支持五级！");
                            return false;
                        }
                        //添加数据
                        d.children.push({
                            id: ++this.maxexpandId,
                            name: '新增节点',
                            pid: d.id,
                            isEdit: false,
                            children: []
                        });
                        //展开节点
                        if (!n.expanded) {
                            n.expanded = true;
                        }
                    },
                    handleEdit(s, d, n) { //编辑节点
                        console.log(s, d, n)
                    },
                    handleDelete(s, d, n) { //删除节点
                        console.log(s, d, n)
                        let that = this;
                        //有子级不删除
                        if (d.children && d.children.length !== 0) {
                            this.$message.error("此节点有子级，不可删除！")
                            return false;
                        } else {
                            //新增节点直接删除，否则要询问是否删除
                            let delNode = () => {
                                let list = n.parent.data.children || n.parent.data, //节点同级数据
                                        _index = 99999; //要删除的index
                                /*if(!n.parent.data.children){//删除顶级节点，无children
                                 list = n.parent.data
                                 }*/
                                list.map((c, i) => {
                                    if (d.id == c.id) {
                                    _index = i;
                                }
                            })
                            let k = list.splice(_index, 1);
                            //console.log(_index,k)
                            this.$message.success("删除成功！")
                        }
                        let isDel = () => {
                            that.$confirm("是否删除此节点？", "提示", {
                                confirmButtonText: "确认",
                                cancelButtonText: "取消",
                                type: "warning"
                            }).then(() => {
                                delNode()
                            }).catch(() => {
                                return false;
                        })
                    }
                    //判断是否新增
                    d.id > this.non_maxexpandId ? delNode() : isDel();

                    }
                    },
                    }

                    })
                </script>     
                    
                    
                    
                    
                    
                    
                    
                    
                    
                    
                    
                    
                    
                    
                    
