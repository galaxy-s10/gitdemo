<template>
  <div>
    <Tree :data="roleList" :render="renderContent" show-checkbox></Tree>
  </div>
</template>

<script>
import { getRoleList } from "../../../api/auth";
export default {
  components: {},
  data() {
    return {
      roleList: [],
    };
  },
  computed: {},
  methods: {
    renderContent(h, { root, node, data }) {
      return h(
        "span",
        {
          style: {
            display: "inline-block",
            width: "100%",
          },
        },
        [
          h("span", [h("span", data.role_name)]),
          h("span", {
            style: {
              display: "inline-block",
              float: "right",
              marginRight: "32px",
            },
          }),
        ]
      );
    },
  },
  created() {
    getRoleList().then((res) => {
      let { rows } = res;
      function handleRole(data) {
        let temp = [];
        data.forEach((item) => {
          if (item.p_id == 0) {
            temp.push(item);
          }
        });
        function digui(data, temp) {
          temp.forEach((tempItem, tempIndex) => {
            let children = [];
            data.forEach((dataItem, dataIndex) => {
              if (tempItem.id == dataItem.p_id) {
                // let children = tempItem.children ? tempItem.children : [];
                children.push(dataItem);
              }
              // if (children.length > 0) {
              //   digui(dataItem[1]);
              // }
            });
            console.log(tempItem);
            if (children.length > 0) {
              tempItem.children = children;
              digui(data, children);
            }
          });
        }
        digui(data, temp);
        return temp;
      }
      this.roleList = handleRole(rows);
    });
  },
  mounted() {},
};
</script>

<style scoped>
</style>
