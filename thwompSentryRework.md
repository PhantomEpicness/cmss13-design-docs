<!-- It's not necessary to follow this format, as long as you provide a coherent and structured document -->

## Abstract
In their current state, sentries as essentially techwebs remnants 
<!-- An abstract is a short blurb, about a paragraph or two, succinctly describing your feature. This should mostly be "why", but can include "what". -->

## Goals

<!-- This is a numbered list clearly detailing your goals for the feature. As per usual, this should be a mixture of both why and what. -->

## Non-goals
/datum/action/xeno_action/onclick/crusher_shield/use_ability(atom/Tar)
	var/mob/living/carbon/Xenomorph/X = owner

	if (!istype(X))
		return

	if (!action_cooldown_check())
		return

	if (!X.check_state())
		return

	if (!check_and_use_plasma_owner())
		return

	X.visible_message(SPAN_XENOWARNING("[X] hunkers down and bolsters its defenses!"), SPAN_XENOHIGHDANGER("You hunker down and bolster your defenses!"))

	X.create_crusher_shield()

	X.add_xeno_shield(shield_amount, XENO_SHIELD_SOURCE_CRUSHER, /datum/xeno_shield/crusher)
	X.overlay_shields()

	X.explosivearmor_modifier += 1000
	X.recalculate_armor()

	addtimer(CALLBACK(src, .proc/remove_explosion_immunity), 25, TIMER_UNIQUE)
	addtimer(CALLBACK(src, .proc/remove_shield), 70, TIMER_UNIQUE)

	apply_cooldown()
	..()
	return

/datum/action/xeno_action/onclick/crusher_shield/proc/remove_explosion_immunity()
	var/mob/living/carbon/Xenomorph/X = owner
	if (!istype(X))
		return

	X.explosivearmor_modifier -= 1000
	X.recalculate_armor()
	to_chat(X, SPAN_XENODANGER("Your immunity to explosion damage ends!"))

/datum/action/xeno_action/onclick/crusher_shield/proc/remove_shield()
	var/mob/living/carbon/Xenomorph/X = owner
	if (!istype(X))
		return

	var/datum/xeno_shield/found
	for (var/datum/xeno_shield/XS in X.xeno_shields)
		if (XS.shield_source == XENO_SHIELD_SOURCE_CRUSHER)
			found = XS
			break

	if (istype(found))
		found.on_removal()
		qdel(found)
		to_chat(X, SPAN_XENOHIGHDANGER("You feel your enhanced shield end!"))

	X.overlay_shields()

<!-- Just like goals, but the opposite! Every feature has boundaries it won't step over. These should be written as if they start with "We will not...". -->

## Content

<!-- Now's where you get into clear detail about everything your feature does. **You should still be explaining 'why' things are that way, *as* you describe what.** Be as detailed as possible. -->

## Alternatives

<!-- Provide potential alternatives to your feature, either ones that align with your design values, or ones that don't that you suspect will be suggested. If you are including the latter, make sure to explain why you didn't choose that. -->

## Potential Changes

<!-- Most of the time you're not going to get the best design first try. It helps to try your best to predict what *could* go wrong, and suggest alternatives that can be taken, without sacrificing your design. -->
