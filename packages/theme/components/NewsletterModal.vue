<template>
  <SfModal
    :visible="isNewsletterModalOpen"
    class="modal"
    @close="closeModal"
  >
    <template #modal-bar>
      <SfBar
        class="modal__title smartphone-only"
        :close="true"
        :title="$t('components.newsletter_modal.subscribe_to_newsletter')"
        @click:close="closeModal"
      />
    </template>
    <transition name="sf-fade" mode="out-in">
      <div>
        <SfHeading
          :level="3"
          :title="$t('components.newsletter_modal.subscribe_to_newsletter')"
          class="modal__title desktop-only"
        />
        <form @submit.prevent="$emit('email-submitted', emailAddress)">
          <SfInput
            type="email"
            :label="$t('components.newsletter_modal.email_address')"
            v-model="emailAddress"
            class="modal__input"
          />
          <SfButton class="modal__button" type="submit">
            {{ $t('components.newsletter_modal.i_confirm_subscription') }}
          </SfButton>
        </form>
        <SfHeading
          :description="$t('components.newsletter_modal.you_can_unsubscribe_at_any_time')"
          :level="3"
        />
        <SfScrollable maxContentHeight="3.75rem" :class="{ 'is-open': !isHidden }">
          <i18n tag="p" class="modal__content" path="components.newsletter_modal.subscribe_to_newsletter_modal_content">
            <SfLink link="https://www.vuestorefront.io/privacy-policy">{{ $t('components.newsletter_modal.privacy_policy') }}</SfLink>
          </i18n>
          <template #view-all>
            <SfButton
              class="sf-button--text sf-scrollable__view-all desktop-only"
              @click="isHidden = !isHidden"
            >
              <span>{{ isHidden ? $t('components.newsletter_modal.show_more') : $t('components.newsletter_modal.hide') }}</span>
            </SfButton>
          </template>
        </SfScrollable>
      </div>
    </transition>
  </SfModal>
</template>
<script>
import { SfModal, SfHeading, SfInput, SfButton, SfScrollable, SfBar, SfLink } from '@storefront-ui/vue';
import { ref } from '@nuxtjs/composition-api';
import { useUiState } from '~/composables';

export default {
  name: 'NewsletterModal',
  components: {
    SfModal,
    SfHeading,
    SfInput,
    SfButton,
    SfScrollable,
    SfBar,
    SfLink
  },
  setup() {
    const { isNewsletterModalOpen, toggleNewsletterModal } = useUiState();

    const isHidden = ref(true);
    const emailAddress = ref('');

    const closeModal = () => {
      toggleNewsletterModal();
    };

    return {
      isNewsletterModalOpen,
      toggleNewsletterModal,
      isHidden,
      emailAddress,
      closeModal
    };
  }
};
</script>

<style lang="scss" scoped>

.modal {
  display: flex;
  justify-content: center;
  --modal-index: 3;
  --overlay-z-index: 3;
  --modal-content-padding: var(--spacer-xl);
  &__input,
  .sf-input__label {
    --input-font-size: var(--font-size--base);
    --input-label-font-size: var(--font-size--base);
  }
  &__button {
    margin: 0 auto;
  }
  &__content {
    font-size: var(--font-size--sm);
    font-weight: var(--font-weight--light);
  }
  .sf-scrollable__view-all.sf-button {
    font-weight: var(--font-weight--light);
  }
}

</style>