<template>
    <Base/>
    <main class="main">
        <!-- Side Nav Dummy-->
        <div class="main__sideNav"></div>
        <!-- End Side Nav -->
        <!-- Main Content -->
        <div class="main__content">
            <!--==================== PROJECTS ====================-->
            <section class="projects section" id="projects">
                <div class="skills_container">
                    <div class="titlebar">
                        <div class="titlebar_item">
                            <h1>Projects </h1>
                        </div>
                        <div class="titlebar_item">
                            <div class="btn btn__open--modal" @click="newProject">
                                New Project
                            </div>
                        </div>
                    </div>

                    <div class="table">

                        <div class="table_filter">
                            <span class="table_filter-Btn ">
                                <i class="fas fa-ellipsis-h"></i>
                            </span>
                            <div>
                                <ul class="table_filter-list">
                                    <li>
                                        <p class="table_filter-link table_filter-link--active">
                                            All
                                        </p>
                                    </li>
                                </ul>
                            </div>
                        </div>

                        <div class="table_search">
                            <div class="table_search-wrapper">
                                <select class="table_search-select" name="" id="">
                                    <option value="">Filter</option>
                                </select>
                            </div>
                            <div class="relative">
                                <i class="table_search-input--icon fas fa-search "></i>
                                <input class="table_search--input" type="text" placeholder="Search Project">
                            </div>
                        </div>

                        <div class="project_table-heading">
                            <p>Image</p>
                            <p>Title</p>
                            <p>Description</p>
                            <p>Link</p>
                            <p>Actions</p>
                        </div>
                        <!-- item 1 -->
                        <div class="project_table-items" v-for="item in projects" :key="item.id" v-if="projects.length > 0">
                            <p>
                                <img :src="myImage(item.photo)"  class="project_img-list">
                            </p>
                            <p>{{item.title}}</p>
                            <p>{{item.description}}</p>
                            <p>{{item.link}}</p>
                            <div>
                                <button class="btn-icon success" @click="onEdit(item.id)">
                                    <i class="fas fa-pencil-alt"></i>
                                </button>
                                <button class="btn-icon danger" @click="deleteProject(item.id)">
                                    <i class="far fa-trash-alt"></i>
                                </button>
                            </div>
                        </div>

                    </div>

                </div>
            </section>

        </div>
    </main>
</template>

<script>
import Base from "../layouts/base.vue";
import {onMounted, ref} from "vue";
import Axios from "../../axios";
import {useRouter} from "vue-router";
export default {
    name: "index",
    components : {Base},

    setup() {
        const router = useRouter()
        const projects = ref([])

        const myImage = (img) => {
           return "/img/upload/"+img
        }

        const newProject = () => {
          router.push('/admin/projects/new')
        }
        const onEdit = (id) => {
          router.push('/admin/projects/edit/'+id)
        }

        const deleteProject = (id) => {
            Swal.fire({
                title: 'Are you sure to delete?',
                text: "you can't go back",
                icon: "warning",
                showCancelButton: true,
                confirmButtonColor: '#3085d6',
                cancelButtonColor: '#d33',
                confirmButtonText: 'Yes, delete it !'
            })
                .then((result) => {
                    if (result.value) {
                        Axios.get('delete-project/' +id).then(()=>{
                            Swal.fire({
                                icon : "success",
                                title : "Deleted",
                                text : "Project deleted successfully"
                            })
                            getProjects()
                        })
                    }
                })
        }

        const getProjects = async () => {
          await Axios.get('get-all-projects').then(res =>{
              projects.value = res.data.projects
          })
        }
        onMounted(async ()=> {
            getProjects()
        })

        return{
            projects,
            myImage,
            newProject,
            onEdit,
            deleteProject
        }
    }
}
</script>

<style scoped>

</style>
