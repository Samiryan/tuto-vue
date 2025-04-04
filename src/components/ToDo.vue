import { ref, onMounted } from 'vue';
import { supabase } from '../supabase';

export default {
  setup() {
    const taches = ref([]);
    const nouvelleTache = ref('');
    const utilisateur = ref(null);

    async function fetchTaches() {
      const { data, error } = await supabase
        .from('taches')
        .select('*')
        .eq('utilisateur_id', utilisateur.value.id);
      if (error) console.error(error);
      else taches.value = data;
    }

    async function ajouterTache() {
      if (!nouvelleTache.value.trim()) return;
      const { data, error } = await supabase.from('taches').insert([
        { nom: nouvelleTache.value, termine: false, utilisateur_id: utilisateur.value.id },
      ]);
      if (!error) taches.value.push(data[0]);
      nouvelleTache.value = '';
    }

    async function supprimerTache(id) {
      await supabase.from('taches').delete().eq('id', id);
      taches.value = taches.value.filter(t => t.id !== id);
    }

    async function toggleTache(id, termine) {
      await supabase.from('taches').update({ termine: !termine }).eq('id', id);
      const tache = taches.value.find(t => t.id === id);
      if (tache) tache.termine = !tache.termine;
    }

    onMounted(async () => {
      const { data: user } = await supabase.auth.getUser();
      if (user) {
        utilisateur.value = user;
        fetchTaches();
      }
    });

    return { taches, nouvelleTache, ajouterTache, supprimerTache, toggleTache };
  }
};
