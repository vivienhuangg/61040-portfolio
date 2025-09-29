## Problem Statement

### Domain: Keeping Track of Recipes

Cooking has become an essential part of my daily life, first in a cook-for-yourself dorm and now living off-campus. My inspiration usually comes from friends or social media, but keeping these recipes organized has always been a struggle. Screenshots pile up in my camera roll, TikTok saves get buried, and text messages with recipes quickly disappear. I care about this domain because cooking is not just a necessity, but a creative and social activity — yet the messy way I track recipes undermines the joy and repeatability of it.

### Problem: Adding Notes on Recipes

When I adapt a recipe — for example, using less salt or adding more spice — I have no good way to record those tweaks for the future. Platforms like NYT Cooking and AllRecipes don’t prioritize personal annotation, and my own ad hoc methods (typing notes into my phone or scribbling on paper) are fragmented and hard to reuse. This means I often forget my adjustments, repeat mistakes, or fail to share reliable versions with friends. The result is wasted effort and lost opportunities to build on past cooking successes.

### Stakeholders

* **Home Cooks (Users)** – Regularly cook and want to save personalized tweaks to recipes.
* **Friends & Family** – Receive or share recipes and benefit from trusted, annotated versions.
* **Recipe Creators & Publishers** – Original authors whose work forms the foundation for user adaptations.

### Evidence & Comparables

* **College Students Cook Often** – Study shows students frequently cook, supporting demand for recipe management [link](https://www.sciencedirect.com/science/article/abs/pii/S1878450X21000020).
* **Cooking Education Boosts Skills** – Education increases cooking frequency, making recall of past tweaks even more valuable [link](https://pmc.ncbi.nlm.nih.gov/articles/PMC10563711/).
* **Home Cooking and Diet Quality** – Research links home cooking to better diets, so better note-taking supports health outcomes [link](https://pmc.ncbi.nlm.nih.gov/articles/PMC8728746/).
* **Food Waste Statistics – USDA** – Mistakes in cooking contribute to food waste; saving successful tweaks reduces waste [link](https://www.usda.gov/foodwaste/faqs).
* **NYT Cooking Notes** – Shows annotation demand, but limited to one ecosystem [link](https://cooking.nytimes.com/).
* **Paprika Recipe Manager** – Rich features but clunky sharing [link](https://www.paprikaapp.com/).
* **AnyList** – Strong for shared lists but not for evolving recipe notes [link](https://www.anylist.com/).
* **Samsung Food** – Focused on saving recipes, less on personal version history [link](https://www.samsungfood.com/).

---

## Application Pitch

### Name

**Nibble**

### Motivation

Cooks constantly tweak recipes — adjusting flavors, substituting ingredients, or modifying steps — but have no easy way to record and reuse those personal changes.

### Key Features

* **Inline Recipe Annotations** – Users can highlight specific ingredients or steps and attach notes (e.g., “use ½ tsp instead of 1 tsp salt”). This keeps improvements tied directly to the recipe, reducing the chance of repeating mistakes.
* **Version Control for Recipes** – Each time a user makes changes, the app saves a new version with notes on the outcome. This creates a living history of reliable variations while preserving the original recipe. Friends and family can easily adopt tested adaptations.
* **Shared Recipe Notebooks** – Users can collect and share annotated recipes in collaborative notebooks with friends or family. This fosters community cooking and ensures everyone benefits from each other’s tweaks, making group meals easier and more enjoyable.

**Nibble** empowers home cooks to build on their own experiences, reduce wasted effort, and share reliable recipes with others — turning messy screenshots and forgotten tweaks into a clear, evolving cooking journal.

# Concept Design

## Concept 1: Recipe

**Purpose**
Represent a base recipe, including ingredients, steps, and metadata, as the foundation for cooking and adaptation.

**Principle**
A recipe should capture its original content faithfully, while supporting links to adaptations and notes.

**State (SSF)**

```
a set of Recipes with
  a title String
  an author String
  a created Date
  a list of Ingredients
  a list of Steps
```

**Actions**

* `createRecipe(title, author, ingredients, steps)`
* `viewRecipe(recipe)`
* `deleteRecipe(recipe)`

---

## Concept 2: Annotation

**Purpose**
Allow cooks to attach notes to specific parts of a recipe (ingredient or step).

**Principle**
Annotations are personal, contextual modifications that enrich the original recipe without overwriting it.

**State (SSF)**

```
a set of Annotations with
  a text String
  a created Date
  an author User
  a target Recipe
  an optional ingredient Ingredient
  an optional step Step
```

**Actions**

* `addAnnotation(recipe, text, target)`
* `editAnnotation(annotation, newText)`
* `removeAnnotation(annotation)`

---

## Concept 3: Version

**Purpose**
Record a snapshot of a recipe with modifications, creating a history of evolving adaptations.

**Principle**
Each version preserves the original recipe while capturing a user’s modifications and outcomes.

**State (SSF)**

```
a set of Versions with
  a base Recipe
  a created Date
  a author User
  a notes String
  a list of Ingredients
  a list of Steps
```

**Actions**

* `createVersion(recipe, modifications, notes)`
* `viewVersion(version)`
* `deleteVersion(version)`

---

## Concept 4: Notebook

**Purpose**
Provide collaborative collections of recipes, versions, and annotations for groups of users.

**Principle**
A notebook enables shared discovery and collective improvement of cooking practices.

**State (SSF)**

```
a set of Notebooks with
  a title String
  an owner User
  a set of Recipes
  a set of Versions
  a set of Members
```

**Actions**

* `createNotebook(title, owner)`
* `addRecipe(notebook, recipe)`
* `shareNotebook(notebook, member)`
* `removeNotebook(notebook)`

---

# Essential Synchronizations

1. **Link annotations to versions**

```
when an Annotation is created
where the Annotation targets a Recipe
then associate the Annotation with the most recent Version of that Recipe
```

2. **Propagate recipe into notebook**

```
when a Recipe is added to a Notebook
then all Versions of that Recipe are also visible in that Notebook
```

3. **Notify members of new version**

```
when a Version is created
where the Version belongs to a Recipe in a Notebook
then notify all Members of the Notebook
```
---

# Notes on Concept Roles

* **Recipe** is the core object, grounding all functionality.
* **Annotation** attaches tweaks directly to recipe components, independent of who authored the recipe.
* **Version** ensures personal and group adaptations are preserved in a timeline, enabling “forks” of recipes without overwriting the original.
* **Notebook** introduces collaboration, allowing multiple users to contribute, view, and share annotated recipes.

Generics:

* The `User` type is instantiated as app users.
* `Ingredient` and `Step` are primitive structural sub-objects belonging to recipes and versions.
* Synchronizations ensure independence: Annotations don’t need to “know” about Notebooks; Notebooks simply surface versions/annotations by association.

---

## User Journey

**Trigger**
It’s a Sunday afternoon, and Vivien and her roommates want to bring dessert to a potluck. They remember that their friend Saph had made *Matcha Brownies* before and saved the recipe in their shared cookbook. Rather than searching through screenshots or old texts, they open the app.

---

**Step 1: Browsing the Cookbook**

![Cookbook View](assignment2images/cookbook_view.png)

Vivien navigates to the **Cookbook View**.. In the sidebar, she sees all her cookbooks — *Family Recipes*, *Cocktails!!!*, and *WIP…*. She clicks on *Roommate Meals*, which opens up a grid of recipe cards. Alongside their *Steak* dinner recipe, she spots *Matcha Brownies*, tagged as *dessert, matcha, potluck*. The tags make it easy to find the right recipe for the occasion.


---

**Step 2: Opening the Recipe**

![Recipe View](assignment2images/recipe_view.png)
Clicking on the card brings Vivien into the **Recipe View** (see *Recipe View sketch*). The recipe title *Matcha Brownies* is at the top, with the header showing it belongs to *Roommate Meals*. The version number reads **3.7**, authored by *Saph* on *8/28/2025*, and the recipe is forked twice. To the right, she sees it’s currently shared with their apartment group *APT 511*.
Scrolling down, Vivien notices a previous comment she made on the ingredient *“1 tsp vanilla extract”* — *“Vanilla Bourbon Paste is better!!!”* (9/28/2025). There’s also a question annotation from their friend Bob on one of the steps. These embedded notes remind the group of personal tweaks and open discussions, instead of cluttered chats.

---

**Step 3: Exploring the Version History**

![Version View](assignment2images/version_view.png)
Before baking, Vivien clicks into the **Version View** (see *Version View sketch*). On the right-hand panel, she sees *Version 3.7 — “Gluten-Free version!”* created earlier that day. The highlights in the ingredient and recipe sections make the changes clear compared to the previous version — a different type of flour, slight adjustments in baking time. Now Vivien can decide whether to use the gluten-free fork or stick with the standard recipe.

---

**Step 4: Cooking and Contributing**
The roommates follow the gluten-free version for the potluck. After baking, Vivien clicks “+ New Version” to record their outcome. She tags it as *potluck hit* and notes, *“Cut sugar by 20%, still delicious.”* This becomes Version 3.8, instantly visible in the *Roommate Meals* cookbook for everyone in APT 511.

---

**Outcome**
Instead of repeating old mistakes or losing track of tweaks, Vivien and her roommates have a shared, evolving record of their favorite recipes. Annotations capture tips and questions, version history highlights adaptations, and cookbooks organize everything by context. What once was a messy thread of screenshots is now a collaborative cooking journal that makes group meals smoother and more fun.


