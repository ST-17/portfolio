<template>
  <div>
    <article
      class="prose dark:prose-invert prose-pre:bg-gray-200 dark:prose-pre:bg-gray-800 prose-pre:text-gray-700 dark:prose-pre:text-gray-300 max-w-none"
    >
      <ContentDoc>
        <template #not-found>
          <h1>Document not found</h1>
          <p>This blog post could not be found</p>
        </template>
        <template v-slot="{ doc }">
          <div class="grid grid-cols-6 gap-16">
            <div :class="{ 'col-span-6 md:col-span-4': doc.toc, 'col-span-6': !doc.toc }">
              <ContentRenderer :value="doc" />
            </div>
            <div class="hidden md:block md:col-span-2 not-prose" v-if="doc.toc">
              <aside class="sticky top-8">
                <div class="font-semibold mb-2">Table of Content</div>
                <nav>
                  <toc-links
                    :links="doc.body.toc.links"
                    :active-id="activeId"
                  ></toc-links>
                </nav>
              </aside>
            </div>
          </div>
        </template>
      </ContentDoc>
    </article>
  </div>
</template>

<script setup>
const activeId = ref(null);
onMounted(() => {
  const callback = (entries) => {
    for (const entry of entries) {
      if (entry.isIntersecting) {
        activeId.value = entry.target.id;
        break;
      }
    }
  };
  const observer = new IntersectionObserver(callback, {
    threshold: 0.5,
    root: null,
  });

  let elements = [];
  elements = document.querySelectorAll("h2, h3");

  for (const el of elements) {
    observer.observe(el);
  }
  onBeforeUnmount(() => {
    for (const el of elements) {
      observer.unobserve(el);
    }
  });
});
</script>
