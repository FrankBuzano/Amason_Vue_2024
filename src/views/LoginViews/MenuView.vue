<template>
  <div class="menu-view">
    <div class="categories-card">
      <CategoryCard
      v-for="(category, index) in categories"
      :key="index"
      :category="category"
      @category-selected="goToProductList"
    />
    </div>
  </div>
</template>

<script>
import CategoryCard from '@/components/MenuComponents/CategoryCard.vue';
import api from '../../../services/api';

export default {
  components: {
    CategoryCard
  },
  data() {
    return {
      categories: []
    }
  },
  methods: {
    async fetchCategories() {
      try {
        const response = await api.get('/categories');
        if (response.status === 200) {
          this.categories = response.data.map(category => {
            // Agregar 4 items quemados
            const dummyItems = [
              { title: 'Celular', name: 'item1', img: 'item1.jpg' },
              { title: 'Laptop', name: 'item2', img: 'item2.jpg' },
              { title: 'Audifonos', name: 'item3', img: 'item3.jpg' },
              { title: 'Camaras', name: 'item4', img: 'item4.jpg' }
            ];
            return {
              ...category,
              items: dummyItems // Asignar los items a la categoría
            };
          });
        }
      } catch (error) {
        console.error('Error fetching categories:', error);
      }
    },
    goToProductList(category) {
      this.$router.push({
        name: 'ProductList',
        query: { categoryId: category.id, title: category.name }
      });
    }
  },
  mounted() {
    this.fetchCategories();
  }
}
</script>

<style scoped>
.categories-card {
  display: flex;
  margin-left: 1vh;
  margin-right: 1vh;
  margin-bottom: 6vh;
  flex-wrap: wrap;
  width: auto;
  background-color: transparent;
  padding: 2.4% 3%;
  gap: 20px;
  justify-content: space-around;
  overflow: hidden;
}
.menu-view {
  display: flex;
  flex-direction: column;
  min-height: 66vh;
}
</style>
