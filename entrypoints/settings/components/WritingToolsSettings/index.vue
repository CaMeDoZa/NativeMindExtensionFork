<template>
  <div
    ref="settingsRef"
    class="flex flex-col font-inter"
  >
    <BlockTitle
      :title="t('settings.writing_tools.title')"
      :description="t('settings.writing_tools.description')"
    />
    <div class="flex flex-col gap-4">
      <Block :title="t('settings.writing_tools.basic_config.title')">
        <div class="flex flex-col gap-4">
          <div class="flex gap-2 items-start">
            <div class="p-1">
              <Checkbox v-model="enabledWritingTools" />
            </div>
            <div>
              <Text
                display="block"
                size="xs"
              >
                {{ t('settings.writing_tools.basic_config.enable') }}
              </Text>
              <Text
                display="block"
                color="secondary"
                size="xs"
              >
                {{ t('settings.writing_tools.basic_config.enable_desc') }}
              </Text>
            </div>
          </div>
        </div>
      </Block>
      <Block
        :title="t('settings.writing_tools.tool_config.title')"
        :disabled="!enabledWritingTools"
      >
        <div class="flex flex-col gap-4">
          <!-- Rewrite -->
          <div class="flex flex-col gap-2">
            <div class="flex gap-2 items-start">
              <div class="p-1">
                <Checkbox v-model="enableRewrite" />
              </div>
              <div class="flex-1">
                <div class="flex gap-[10px] items-center flex-wrap">
                  <Text
                    display="block"
                    size="xs"
                  >
                    {{ t('settings.writing_tools.tool_config.rewrite.title') }}
                  </Text>
                  <WarningMessage
                    v-if="rewriteErrorMessage"
                    :message="rewriteErrorMessage"
                  />
                </div>
                <Text
                  display="block"
                  color="secondary"
                  size="xs"
                >
                  {{ t('settings.writing_tools.tool_config.rewrite.description') }}
                </Text>
                <div class="flex flex-col gap-2 pl-1 pt-2">
                  <div class="flex items-center gap-3">
                    <label class="text-xs text-text-secondary whitespace-nowrap min-w-[80px]">{{ t('settings.writing_tools.tool_config.temperature') }}</label>
                    <Input
                      v-model.number="rewriteTemperature"
                      type="number"
                      min="0"
                      max="1"
                      step="0.05"
                      class="rounded-md py-1.5 px-2 w-20 text-xs"
                    />
                    <input
                      type="range"
                      min="0"
                      max="1"
                      step="0.05"
                      class="flex-1 h-1 accent-[var(--color-accent)]"
                      :value="rewriteTemperature"
                      @input="rewriteTemperature = parseFloat(($event.target as HTMLInputElement).value)"
                    >
                  </div>
                  <Text
                    color="secondary"
                    size="xs"
                  >
                    {{ t('settings.writing_tools.tool_config.temperature_desc') }}
                  </Text>
                  <div class="flex items-center gap-2">
                    <Checkbox v-model="rewriteReasoningEnabled" />
                    <div>
                      <Text size="xs">
                        {{ t('settings.writing_tools.tool_config.enable_reasoning') }}
                      </Text>
                      <Text
                        color="secondary"
                        size="xs"
                      >
                        {{ t('settings.writing_tools.tool_config.enable_reasoning_desc') }}
                      </Text>
                    </div>
                  </div>
                </div>
                <SavedMessage :watch="[rewriteTemperature, rewriteReasoningEnabled]" />
              </div>
            </div>
          </div>

          <!-- Proofread -->
          <div class="flex flex-col gap-2">
            <div class="flex gap-2 items-start">
              <div class="p-1">
                <Checkbox v-model="enableProofread" />
              </div>
              <div class="flex-1">
                <div class="flex gap-[10px] items-center flex-wrap">
                  <Text
                    display="block"
                    size="xs"
                  >
                    {{ t('settings.writing_tools.tool_config.proofread.title') }}
                  </Text>
                  <WarningMessage
                    v-if="proofreadErrorMessage"
                    :message="proofreadErrorMessage"
                  />
                </div>
                <Text
                  display="block"
                  color="secondary"
                  size="xs"
                >
                  {{ t('settings.writing_tools.tool_config.proofread.description') }}
                </Text>
                <div class="flex flex-col gap-2 pl-1 pt-2">
                  <div class="flex items-center gap-3">
                    <label class="text-xs text-text-secondary whitespace-nowrap min-w-[80px]">{{ t('settings.writing_tools.tool_config.temperature') }}</label>
                    <Input
                      v-model.number="proofreadTemperature"
                      type="number"
                      min="0"
                      max="1"
                      step="0.05"
                      class="rounded-md py-1.5 px-2 w-20 text-xs"
                    />
                    <input
                      type="range"
                      min="0"
                      max="1"
                      step="0.05"
                      class="flex-1 h-1 accent-[var(--color-accent)]"
                      :value="proofreadTemperature"
                      @input="proofreadTemperature = parseFloat(($event.target as HTMLInputElement).value)"
                    >
                  </div>
                  <Text
                    color="secondary"
                    size="xs"
                  >
                    {{ t('settings.writing_tools.tool_config.temperature_desc') }}
                  </Text>
                  <div class="flex items-center gap-2">
                    <Checkbox v-model="proofreadReasoningEnabled" />
                    <div>
                      <Text size="xs">
                        {{ t('settings.writing_tools.tool_config.enable_reasoning') }}
                      </Text>
                      <Text
                        color="secondary"
                        size="xs"
                      >
                        {{ t('settings.writing_tools.tool_config.enable_reasoning_desc') }}
                      </Text>
                    </div>
                  </div>
                </div>
                <SavedMessage :watch="[proofreadTemperature, proofreadReasoningEnabled]" />
              </div>
            </div>
          </div>

          <!-- List -->
          <div class="flex flex-col gap-2">
            <div class="flex gap-2 items-start">
              <div class="p-1">
                <Checkbox v-model="enableList" />
              </div>
              <div class="flex-1">
                <div class="flex gap-[10px] items-center flex-wrap">
                  <Text
                    display="block"
                    size="xs"
                  >
                    {{ t('settings.writing_tools.tool_config.list.title') }}
                  </Text>
                  <WarningMessage
                    v-if="listErrorMessage"
                    :message="listErrorMessage"
                  />
                </div>
                <Text
                  display="block"
                  color="secondary"
                  size="xs"
                >
                  {{ t('settings.writing_tools.tool_config.list.description') }}
                </Text>
                <div class="flex flex-col gap-2 pl-1 pt-2">
                  <div class="flex items-center gap-3">
                    <label class="text-xs text-text-secondary whitespace-nowrap min-w-[80px]">{{ t('settings.writing_tools.tool_config.temperature') }}</label>
                    <Input
                      v-model.number="listTemperature"
                      type="number"
                      min="0"
                      max="1"
                      step="0.05"
                      class="rounded-md py-1.5 px-2 w-20 text-xs"
                    />
                    <input
                      type="range"
                      min="0"
                      max="1"
                      step="0.05"
                      class="flex-1 h-1 accent-[var(--color-accent)]"
                      :value="listTemperature"
                      @input="listTemperature = parseFloat(($event.target as HTMLInputElement).value)"
                    >
                  </div>
                  <Text
                    color="secondary"
                    size="xs"
                  >
                    {{ t('settings.writing_tools.tool_config.temperature_desc') }}
                  </Text>
                  <div class="flex items-center gap-2">
                    <Checkbox v-model="listReasoningEnabled" />
                    <div>
                      <Text size="xs">
                        {{ t('settings.writing_tools.tool_config.enable_reasoning') }}
                      </Text>
                      <Text
                        color="secondary"
                        size="xs"
                      >
                        {{ t('settings.writing_tools.tool_config.enable_reasoning_desc') }}
                      </Text>
                    </div>
                  </div>
                </div>
                <SavedMessage :watch="[listTemperature, listReasoningEnabled]" />
              </div>
            </div>
          </div>

          <!-- Sparkle -->
          <div class="flex flex-col gap-2">
            <div class="flex gap-2 items-start">
              <div class="p-1">
                <Checkbox v-model="enableSparkle" />
              </div>
              <div class="flex-1">
                <div class="flex gap-[10px] items-center flex-wrap">
                  <Text
                    display="block"
                    size="xs"
                  >
                    {{ t('settings.writing_tools.tool_config.sparkle.title') }}
                  </Text>
                  <WarningMessage
                    v-if="sparkleErrorMessage"
                    :message="sparkleErrorMessage"
                  />
                </div>
                <Text
                  display="block"
                  color="secondary"
                  size="xs"
                >
                  {{ t('settings.writing_tools.tool_config.sparkle.description') }}
                </Text>
                <div class="flex flex-col gap-2 pl-1 pt-2">
                  <div class="flex items-center gap-3">
                    <label class="text-xs text-text-secondary whitespace-nowrap min-w-[80px]">{{ t('settings.writing_tools.tool_config.temperature') }}</label>
                    <Input
                      v-model.number="sparkleTemperature"
                      type="number"
                      min="0"
                      max="1"
                      step="0.05"
                      class="rounded-md py-1.5 px-2 w-20 text-xs"
                    />
                    <input
                      type="range"
                      min="0"
                      max="1"
                      step="0.05"
                      class="flex-1 h-1 accent-[var(--color-accent)]"
                      :value="sparkleTemperature"
                      @input="sparkleTemperature = parseFloat(($event.target as HTMLInputElement).value)"
                    >
                  </div>
                  <Text
                    color="secondary"
                    size="xs"
                  >
                    {{ t('settings.writing_tools.tool_config.temperature_desc') }}
                  </Text>
                  <div class="flex items-center gap-2">
                    <Checkbox v-model="sparkleReasoningEnabled" />
                    <div>
                      <Text size="xs">
                        {{ t('settings.writing_tools.tool_config.enable_reasoning') }}
                      </Text>
                      <Text
                        color="secondary"
                        size="xs"
                      >
                        {{ t('settings.writing_tools.tool_config.enable_reasoning_desc') }}
                      </Text>
                    </div>
                  </div>
                </div>
                <SavedMessage :watch="[sparkleTemperature, sparkleReasoningEnabled]" />
              </div>
            </div>
          </div>
        </div>
      </Block>
    </div>
  </div>
</template>

<script setup lang="tsx">
import { Ref, watch } from 'vue'

import Checkbox from '@/components/Checkbox.vue'
import Input from '@/components/Input.vue'
import Text from '@/components/ui/Text.vue'
import WarningMessage from '@/components/WarningMessage.vue'
import { useTimeoutValue } from '@/composables/useTimeoutValue'
import { useI18n } from '@/utils/i18n'
import { getUserConfig } from '@/utils/user-config'

import Block from '../Block.vue'
import BlockTitle from '../BlockTitle.vue'
import SavedMessage from '../SavedMessage.vue'

const { t } = useI18n()

const userConfig = await getUserConfig()

const enabledWritingTools = userConfig.writingTools.enable.toRef()
const enableProofread = userConfig.writingTools.proofread.enable.toRef()
const enableRewrite = userConfig.writingTools.rewrite.enable.toRef()
const enableList = userConfig.writingTools.list.enable.toRef()
const enableSparkle = userConfig.writingTools.sparkle.enable.toRef()

const rewriteTemperature = userConfig.writingTools.rewrite.temperature.toRef()
const proofreadTemperature = userConfig.writingTools.proofread.temperature.toRef()
const listTemperature = userConfig.writingTools.list.temperature.toRef()
const sparkleTemperature = userConfig.writingTools.sparkle.temperature.toRef()

const rewriteReasoningEnabled = userConfig.writingTools.rewrite.reasoningEnabled.toRef()
const proofreadReasoningEnabled = userConfig.writingTools.proofread.reasoningEnabled.toRef()
const listReasoningEnabled = userConfig.writingTools.list.reasoningEnabled.toRef()
const sparkleReasoningEnabled = userConfig.writingTools.sparkle.reasoningEnabled.toRef()

const { value: rewriteErrorMessage, setValue: setRewriteErrorMessage } = useTimeoutValue<string | undefined>(undefined, undefined, 4000)
const { value: proofreadErrorMessage, setValue: setProofreadErrorMessage } = useTimeoutValue<string | undefined>(undefined, undefined, 4000)
const { value: listErrorMessage, setValue: setListErrorMessage } = useTimeoutValue<string | undefined>(undefined, undefined, 4000)
const { value: sparkleErrorMessage, setValue: setSparkleErrorMessage } = useTimeoutValue<string | undefined>(undefined, undefined, 4000)

const enableGuard = (v: Ref<boolean>, setError: () => void) => {
  watch(v, (enabled) => {
    const enableToolsCount = [enableProofread, enableRewrite, enableList, enableSparkle].filter((v) => v.value).length
    if (enableToolsCount === 0 && !enabled) {
      setError()
      v.value = true
    }
  })
}

enableGuard(enableProofread, () => setProofreadErrorMessage(t('settings.writing_tools.tool_config.at_least_one_tool_error')))
enableGuard(enableRewrite, () => setRewriteErrorMessage(t('settings.writing_tools.tool_config.at_least_one_tool_error')))
enableGuard(enableList, () => setListErrorMessage(t('settings.writing_tools.tool_config.at_least_one_tool_error')))
enableGuard(enableSparkle, () => setSparkleErrorMessage(t('settings.writing_tools.tool_config.at_least_one_tool_error')))
</script>
