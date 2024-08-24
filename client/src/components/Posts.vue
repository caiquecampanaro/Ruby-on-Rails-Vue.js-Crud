<template>
    <div>
        <h1>Posts</h1>

    <!-- Create or Post -->
     <input type="text" v-model="title" placeholder="Title" class="title-input" />  
     <input type="text" v-model="body" placeholder="Body" class="body-input" />  
    
    <!-- only render if editing post -->
    <!--v-if verifica se essa variável esta sendo editada -->
       <button v-if="isEditing" @click="updatePost">Update</button> 
       <button v-if="isEditing" @click="cancelEdit">Cancel</button> 
    
       <!-- only render if editing post -->
       <button v-else @click="createPost">Create</button> 

        <!-- only render if editing post -->
        <div v-for="post in posts" :key="post.id">
                <h5>[{{post.id}}] {{post.title}}</h5>
                <p>{{post.body}}</p>
                <button class="button-edit" @click="editPost(post.id)">Edit</button>
                <button @click="deletePost(post.id)">Delete</button>
        </div> 
    </div>
</template>
    <!--quando async: a função retorna um encapsulado em uma promise ex: Promise.resolve(true) -->
<script setup>
    import { ref, onMounted } from "vue";

    const posts = ref([])
    const title = ref('')
    const body = ref('')
    const post_id = ref(0)
    const isEditing = ref(false)
    const API_URL = "http://localhost:3000/post"

    onMounted(async () => {
      try {
        const response = await fetch(API_URL); // Substitua pela URL correta
        const data = await response.json();
        posts.value = data;
      } catch (error) {
        console.error('Erro ao buscar posts:', error);
      }
    });

    const createPost = async () => {
        const res = await fetch(API_URL, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({
                title: title.value,
                body: body.value
            })
        })

        const data = await res.json()
            
        posts.value.push(data)
        title.value = ''
        body.value = ''
        post_id.value = 0;  /* garantir que n vai ser enviado um ID */
    }

    const updatePost = async () => {
        const res = await fetch(`${API_URL}/${post_id.value}`, {
        method: 'PUT', // Este objeto deve estar entre chaves {}
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify({
            title: title.value,
            body: body.value,
            id: post_id.value
        })
    })
        const data = await res.json()

        const index = posts.value.findIndex(post => post.id === data.id)
        posts.value[index] = data

        title.value = ''
        body.value = ''
        post_id.value = 0;
        isEditing.value = false

    }
    const cancelEdit = () => {
        title.value = ''
        body.value = ''
        post_id.value = 0;
        isEditing.value = false
    }

    const deletePost = async(id) => {
        try {
            // Faça a requisição DELETE para a API
            const response = await fetch(`${API_URL}/${id}`, {
            method: 'DELETE',
            });

            // Verifique se a requisição foi bem-sucedida
            if (!response.ok) {
            throw new Error('Falha ao excluir o post');
            }

            // Atualize a lista de posts filtrando o post excluído
            posts.value = posts.value.filter(post => post.id !== id);
            } catch (error) {
                console.error('Erro ao excluir o post:', error);
            }
    }

    const editPost = async(id) => {
        const post = posts.value.find(post => post.id === id);

        title.value = post.title
        body.value = post.body
        post_id.value = post.id
        isEditing.value = true

        window.scrollTo({
            top: 0,
            behavior: 'smooth'
        })  
    }
</script>

<style scoped>
.title-input {
    width: 100%;
    padding: 12px 20px;
    margin: 8px 0;
    box-sizing: border-box;
    border: 2px solid #ccc;
    background-color: #f8f8f8;
    color: #111;
    border-radius: 4px;
    resize: vertical;
}

.body-input {
    width: 100%;
    padding: 12px 20px;
    margin: 8px 0;
    box-sizing: border-box;
    border: 2px solid #ccc;
    background-color: #f8f8f8;
    color: #111;
    border-radius: 4px;
    resize: vertical;
}

.button-edit{
    margin-right: 10px;
}
</style>