<template>
    <div>
        <h1>Posts</h1>

    <!-- Create or Post -->
     <input type="text" v-model="title" placeholder="Title" class="title-input">  
     <input type="text" v-model="body" placeholder="Body" class="body-input">  
    
    <!-- only render if editing post -->
    <!--v-if verifica se essa variável esta sendo editada -->
       <button v-if="isEditing" @click="updatePost">Update</button> 
       <button v-if="isEditing" @click="cancelEdit">Cancel</button> 
    
       <!-- only render if editing post -->
       <button v-else @click="createPost">Create</button> 
    </div>
</template>
    <!--quando async: a função retorna um encapsulado em uma promise ex: Promise.resolve(true) -->
<script setup>
    const updatePost = async () => {
        return true;
    }
    const cancelEdit = () => {
        return true;
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

</style>