<template>
  <div class="hello" style="text-align:center">
    <el-table :data="HitCount" style="width: 50%">
      <el-table-column type="index" width="50"></el-table-column>
      <el-table-column prop="Hits" label="访问数" width="180"></el-table-column>
      <el-table-column prop="Visitors" label="访客数" width="180"></el-table-column>
      <el-table-column prop="ApiCalls" label="接口调用数"></el-table-column>
    </el-table>
    <br>
     <br>
    <el-table :data="domains" style="width: 50%">
      <el-table-column type="index" width="50"></el-table-column>
      <el-table-column prop="domain" label="主机名" width="180"></el-table-column>
      <el-table-column prop="ip" label="IP" width="180"></el-table-column>
      <el-table-column prop="date" label="更新时间" width="180"></el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      msg: "Welcome to Your Vue.js App",
      domain: "",
      ip: "",
      HitCount: [
        {
          Hits: 34,
          Visitors: 4567,
          ApiCalls: "上海市普陀区金沙江路 1518 弄"
        }
      ],
      domains: []
    };
  },
  beforeCreate() {
    // 调用接口/api/DomainIp
    // this.$api({
    //   method: "get",
    //   url: "/api/DomainIp"
    // })
    //   .then(response => {
    //     console.log(response.data);
    //   })
    //   .catch(function(error) {
    //     console.log(error);
    //   });
    //调用接口/api/DomainIp post
    // this.$api({
    //   method: "post",
    //   url: "/api/DomainIp",
    //   data: JSON.stringify({
    //     domain: "string",
    //     ip: "string",
    //     date: "2020-07-14T08:31:54.587Z"
    //   })
    // })
    //   .then(response => {
    //     console.log(response.data);
    //   })
    //   .catch(function(error) {
    //     console.log(error);
    //   });
    // 调用接口/api/DomainIp/Latest
    // this.$api({
    //   method: "GET",
    //   url: "/api/DomainIp/Latest",
    //   params: {
    //     domain: "vpn.xinyimall.com.cn"
    //   }
    // })
    //   .then(response => {
    //     console.log(response.data);
    //     this.domain = response.data.result.domain;
    //     this.ip = response.data.result.ip;
    //   })
    //   .catch(function(error) {
    //     console.log(error);
    //   });
    // 调用接口/api​/DomainIp​/Ping
    // this.$api({
    //   method: "GET",
    //   url: "/api/DomainIp/Ping",
    //   params: {
    //     hostName: "vpn.xinyimall.com.cn"
    //   }
    // })
    //   .then(response => {
    //     console.log(response.data);
    //   })
    //   .catch(function(error) {
    //     console.log(error);
    //   });
    // 调用接口/api/DomainIp/HostNames
    // this.$api({
    //   method: "GET",
    //   url: "/api/DomainIp/HostNames"
    // })
    //   .then(response => {
    //     console.log(response.data);
    //   })
    //   .catch(function(error) {
    //     console.log(error);
    //   });
  let _this = this;
    // 展示访客数
    this.$api({
      method: "GET",
      url: "/api/HitCount/Hits"
    })
      .then(response => {
        console.log(response.data);
        _this.HitCount[0].Hits = response.data.result
      })
      .catch(function(error) {
        console.log(error);
      });
    // 展示访问数
        this.$api({
      method: "GET",
      url: "/api/HitCount/Visitors"
    })
      .then(response => {
        console.log(response.data);
            _this.HitCount[0].Visitors = response.data.result
        
      })
      .catch(function(error) {
        console.log(error);
      });
      // 展示接口调用次数
        this.$api({
      method: "GET",
      url: "/api/HitCount/ApiCalls"
    })
      .then(response => {
        console.log(response.data);
          _this.HitCount[0].ApiCalls = response.data.result
      })
      .catch(function(error) {
        console.log(error);
      });

    // 展示域名与ip
  
    this.$api({
      method: "GET",
      url: "/api/DomainIp/HostNames"
    })
      .then(response => {
        let data1 = response.data.result.items;
        console.log(data1);
        for (var item of data1) {
          this.$api({
            method: "GET",
            url: "/api/DomainIp/Latest",
            params: {
              domain: item.hostName
            }
          })
            .then(response => {
              let domainItem = {};
              domainItem.domain = response.data.result.domain;
              domainItem.ip = response.data.result.ip;
              domainItem.date = response.data.result.date;
              _this.domains.push(domainItem);
              console.log(response.data);
            })
            .catch(function(error) {
              console.log(error);
            });
        }
      })
      .catch(function(error) {
        console.log(error);
      });
  },
  methods: {
    getHostNames() {
      let _this = this;
      this.$api({
        method: "GET",
        url: "/api/DomainIp/HostNames"
      })
        .then(response => {
          let data1 = response.data.result.items;
          console.log(data1);
          for (var item of data1) {
            this.$api({
              method: "GET",
              url: "/api/DomainIp/Latest",
              params: {
                domain: item.hostName
              }
            })
              .then(response => {
                let domainItem = {};
                domainItem.domain = response.data.result.domain;
                domainItem.ip = response.data.result.ip;
                domainItem.date = response.data.result.date;
                _this.domains.push(domainItem);
                console.log(response.data);
              })
              .catch(function(error) {
                console.log(error);
              });
          }
        })
        .catch(function(error) {
          console.log(error);
        });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
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
