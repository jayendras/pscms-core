<template>
    <el-button type="danger" size="mini" @click="deleteRow"><i class="fa fa-trash"></i></el-button>
</template>

<script>
    export default {
        props: {
            rows: { default: null, type: Array },
            scope: { default: null, type: Object },
        },
        data() {
            return {
                deleteMessage: '',
                deleteTitle: '',
            };
        },
        mounted() {
            this.deleteMessage = this.trans('core.modal.confirmation-message');
            this.deleteTitle = this.trans('core.modal.title');
        },
        methods: {
            deleteRow() {
                this.$confirm(this.deleteMessage, this.deleteTitle, {
                    confirmButtonText: this.trans('core.button.delete'),
                    cancelButtonText: this.trans('core.button.cancel'),
                    type: 'warning',
                    confirmButtonClass: 'el-button--danger',
                }).then(() => {
                    const vm = this;
                    axios.delete(this.scope.row.urls.delete_url)
                        .then((response) => {
                            if (response.data.errors === false) {
                                vm.$message({
                                    type: 'success',
                                    message: response.data.message,
                                });

                                vm.rows.splice(vm.scope.$index, 1);
                            }
                        })
                        .catch((error) => {
                            vm.$message({
                                type: 'error',
                                message: error.data.message,
                            });
                        });
                }).catch(() => {
                    this.$message({
                        type: 'info',
                        message: this.trans('core.delete cancelled'),
                    });
                });
            },
        },
    };
</script>
