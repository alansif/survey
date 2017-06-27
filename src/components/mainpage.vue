<template>
	<div>
      hello
	  <el-button type="primary" @click="go()">click</el-button>
	  <p>{{aabb}}</p>
	</div>
</template>

<script>
    export default {
        data () {
            return {
			    aabb:""
            }
        },
		methods: {
		    go() {
		        this.$http.post("http://111.198.146.40:8082/customize/Survey.asmx/GetAllQuestion", {
                }).then((response) => {
                    console.log(response);
					this.aabb = response.body.d;
                }, (response) => {
                    console.log(response);
                }).catch((response) => {
                    console.log(response);
                });
		    }
        }
	}
</script>
