<script lang="ts">
	import { derived } from "svelte/store";
	import { setCtx } from "../ctx.js";
	import type { Props } from "../types.js";

	type $$Props = Props;

	export let value: $$Props["value"] = undefined;
	export let onValueChange: $$Props["onValueChange"] = undefined;
	export let placeholder: $$Props["placeholder"] = undefined;
	export let onPlaceholderChange: $$Props["onPlaceholderChange"] = undefined;
	export let disabled: $$Props["disabled"] = undefined;
	export let isDateUnavailable: $$Props["isDateUnavailable"] = undefined;
	export let granularity: $$Props["granularity"] = undefined;
	export let hideTimeZone: $$Props["hideTimeZone"] = undefined;
	export let hourCycle: $$Props["hourCycle"] = undefined;
	export let locale: $$Props["locale"] = undefined;
	export let maxValue: $$Props["maxValue"] = undefined;
	export let minValue: $$Props["minValue"] = undefined;
	export let readonly: $$Props["readonly"] = undefined;
	export let validationId: $$Props["validationId"] = undefined;
	export let descriptionId: $$Props["descriptionId"] = undefined;
	export let readonlySegments: $$Props["readonlySegments"] = undefined;

	const {
		states: {
			value: localValue,
			placeholder: localPlaceholder,
			isInvalid: localIsInvalid
		},
		updateOption,
		ids
	} = setCtx({
		defaultValue: value,
		defaultPlaceholder: placeholder,
		disabled,
		granularity,
		hideTimeZone,
		hourCycle,
		locale,
		maxValue,
		minValue,
		readonly,
		readonlySegments,
		isDateUnavailable,
		onValueChange: ({ next }) => {
			if (value !== next) {
				onValueChange?.(next);
				value = next;
			}
			return next;
		},
		onPlaceholderChange: ({ next }) => {
			if (placeholder !== next) {
				onPlaceholderChange?.(next);
				placeholder = next;
			}
			return next;
		}
	});

	const idValues = derived(
		[
			ids.day,
			ids.description,
			ids.dayPeriod,
			ids.field,
			ids.hour,
			ids.minute,
			ids.month,
			ids.second,
			ids.year,
			ids.validation,
			ids.label,
			ids.timeZoneName
		],
		([
			$dayId,
			$descriptionId,
			$dayPeriodId,
			$fieldId,
			$hourId,
			$minuteId,
			$monthId,
			$secondId,
			$yearId,
			$validationId,
			$labelId,
			$timeZoneNameId
		]) => ({
			day: $dayId,
			description: $descriptionId,
			dayPeriod: $dayPeriodId,
			field: $fieldId,
			hour: $hourId,
			minute: $minuteId,
			month: $monthId,
			second: $secondId,
			year: $yearId,
			validation: $validationId,
			label: $labelId,
			timeZoneName: $timeZoneNameId
		})
	);

	$: if (validationId) {
		ids.validation.set(validationId);
	}

	$: if (descriptionId) {
		ids.description.set(descriptionId);
	}

	$: value !== undefined && localValue.set(value);
	$: placeholder !== undefined && localPlaceholder.set(placeholder);

	$: updateOption("disabled", disabled);
	$: updateOption("isDateUnavailable", isDateUnavailable);
	$: updateOption("granularity", granularity);
	$: updateOption("hideTimeZone", hideTimeZone);
	$: updateOption("hourCycle", hourCycle);
	$: updateOption("locale", locale);
	$: updateOption("maxValue", maxValue);
	$: updateOption("minValue", minValue);
	$: updateOption("readonly", readonly);
	$: updateOption("readonlySegments", readonlySegments);
</script>

<slot isInvalid={$localIsInvalid} ids={$idValues} />
