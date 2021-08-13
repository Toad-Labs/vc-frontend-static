<template>
  <li
    tabindex="0"
    @click="selectInboxItem"
    @keyup.enter="selectInboxItem"
    :class="[
      inboxItem.selected
        ? 'bg-blue-selected rounded-none'
        : 'focus:bg-gray-infolt hover:bg-gray-infolt ',
      inboxItem.id ? '' : 'bg-gray-infolt animate-pulse',
      'flex items-center w-full h-16 md:h-20 rounded active:bg-blue-selected cursor-pointer',
    ]"
  >
    <div class="p-1">
      <img
        v-if="inboxItem.senderIcon"
        class="w-12 h-12 md:w-12 md:h-12"
        :src="icons[inboxItem.senderIcon]"
        alt=""
      />
      <div
        v-else
        class="w-12 h-12 md:w-12 md:h-12 bg-gray-infomd rounded-full"
      />
    </div>
    <div class="flex-1 truncate">
      <span :id="'sender-name-label-' + indexNum" class="hidden">
        {{ $t("inboxFor") }}
      </span>
      <h2
        :id="'sender-name-' + indexNum"
        :aria-labelledby="
          'sender-name-label-' + indexNum + ' sender-name-' + indexNum
        "
        :class="[
          !inboxItem.dayRead ? 'font-bold' : '',
          'font-body md:text-lg truncate overflow-ellipsis text-gray-dark pl-1',
        ]"
      >
        {{ inboxItem.senderName }}
      </h2>
      <span class="hidden" :id="'status-' + indexNum"
        >{{
          inboxItem.selected === true
            ? $t("currentlySelected")
            : $t("accessInboxItem") +
              (inboxItem.dayRead ? $t("alreadyRead") : $t("unread"))
        }}.
      </span>
      <span class="hidden" :id="'teaser-announcement-' + indexNum"
        >{{ $t("teaserText") }}:</span
      >
      <h2
        :id="'teaser-text-' + indexNum"
        :aria-labelledby="
          'status-' +
          indexNum +
          ' teaser-announcement-' +
          indexNum +
          ' teaser-text-' +
          indexNum
        "
        :class="[
          !inboxItem.dayRead ? 'font-body' : 'font-heading font-light',
          'text-sm md:text-lg truncate overflow-ellipsis text-gray-dark pl-1',
        ]"
      >
        {{ inboxItem.teaserText ? inboxItem.teaserText : $t("noMessages") }}
      </h2>
    </div>
    <div>
      <read-notification
        v-if="inboxItem.id"
        v-bind:dayRead="inboxItem.dayRead"
        :indexNum="indexNum"
      />
    </div>
  </li>
</template>

<script>
import icons from "../../assets/icons.js";
import ReadNotification from "../atoms/ReadNotification.vue";
export default {
  props: {
    inboxItem: Object,
    indexNum: Number,
  },
  setup(props, context) {
    function selectInboxItem() {
      context.emit("select-inbox-item", props.inboxItem.id);
    }
    return { selectInboxItem, icons };
  },
  components: {
    ReadNotification,
  },
};
</script>
