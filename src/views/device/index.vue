<template>
    <div class="device">
        <div class="content"
             v-loading="loading">
            <el-table :data="tableData"
                      stripe
                      style="width: 100%">
                <el-table-column type="index"
                                 width="50">
                </el-table-column>
                <el-table-column label="设备"
                                 width="80">
                    <template slot-scope="scope">
                        <div class="icon">
                            <svg-icon :iconClass="deviceIcon[scope.row.categoryItemId]" />
                        </div>
                    </template>
                </el-table-column>
                <el-table-column prop="deviceId"
                                 label="DeviceID"
                                 width="200">
                </el-table-column>
                <el-table-column prop="groupId"
                                 label="GroupID">
                </el-table-column>
                <el-table-column prop="name"
                                 label="名称">
                </el-table-column>
                <el-table-column prop="categoryItemId"
                                 label="设备类型">
                </el-table-column>
                <el-table-column prop="desc"
                                 label="介绍">
                </el-table-column>
                <el-table-column prop="roomId"
                                 label="所在位置ID">
                </el-table-column>
                <el-table-column prop="networking"
                                 label="连接方式">
                </el-table-column>
                <el-table-column prop="os"
                                 label="操作系统">
                </el-table-column>
                <el-table-column prop="protocol"
                                 label="通讯时间">
                </el-table-column>
                <el-table-column label="创建时间"
                                 width="200">
                    <template slot-scope="scope">
                        {{ new Date(scope.row.createTime).toLocaleString('zh-CN', { hour12: false }) }}
                    </template>
                </el-table-column>
            </el-table>
        </div>
    </div>
</template>

<script>
import { getDevices } from '@/api/admin';
import { DEVICEICON } from '@/config';
export default {
	name: 'Device', // 设备概览
	data() {
		return {
			loading: false,
			tableData: [],
			deviceIcon: DEVICEICON,
		};
	},

	methods: {
		getDevicesFn() {
			this.loading = true;
			getDevices()
				.then(resData => {
					this.tableData = resData;
				})
				.catch(error => {
					this.$message({
						showClose: true,
						center: true,
						message: error.message,
						type: 'error',
					});
				})
				.then(() => {
					this.loading = false;
				});
		},
	},

	created() {
		this.getDevicesFn();
	},
};
</script>

<style lang="scss" scoped>
.device {
	display: flex;

	.content {
		flex: 1;
		display: flex;
		padding: 20px;
		@include info-card();

		.el-table {
			flex: 1;
		}

		.icon {
			width: 40px;
			height: 40px;
		}
	}
}
</style>
