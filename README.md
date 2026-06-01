## Methodology

This repository analyses long-run changes in immigration framing by identifying recurring lexical patterns in news coverage and aggregating them over time. The aim is not to reduce an article to a single “true meaning,” but to create a systematic first-pass measure of how different rhetorical frames recur, intensify, or decline across years and across outlets.

The analysis suggests that immigration coverage changes over time not only in intensity, but in the kinds of frames that dominate it. By tracing shifts between humanitarian, crisis, control, and integration language across outlets and years, the project highlights how media rhetoric helps shape the terms through which immigration becomes publicly understood, contested, and increasingly normalized through the language of borders, legality, and enforcement.

### 1. Unit of analysis

The primary unit of analysis is the individual article. Each article is treated as one observation in the article-level dataset, while lexical matches are also aggregated by year and outlet to support longitudinal comparison.

This means the project works at two levels at once:
- **Article level**, to identify whether an article contains language associated with one or more framing classes.
- **Yearly aggregate level**, to measure how often particular frames appear in a given outlet over time.

This two-level design makes it possible to compare both individual texts and larger rhetorical patterns. An article may contain multiple frames, while yearly totals show how strongly certain types of framing cluster in particular periods.

### 2. Text used for analysis

For each article, the available text fields are concatenated into a single plain-text analysis field. This field combines:
- headline
- standfirst
- trail text
- body text

Combining these fields ensures that the analysis captures both the compressed framing of headlines and standfirsts and the fuller language used in the article body. This is important because framing often appears most strongly in titles, intros, and repeated descriptive phrases, not only in the main article text.

Before analysis, the text is flattened into a consistent plain-text representation so that lexical matching can be applied systematically across the corpus. Depending on the scraper and source structure, this stage may also involve basic cleaning such as removing null fields, normalizing whitespace, standardizing punctuation, and excluding records with insufficient text.

### 3. Data collection and scraping

The corpus was assembled by collecting article metadata and text from the selected newspaper sources and structuring the results into a consistent tabular dataset. The goal of the scraping stage was to preserve enough information to support both close reading and quantitative aggregation.

For each article, the pipeline typically preserves:
- outlet
- publication date
- year
- title/headline
- standfirst or trail text
- body text
- article URL or identifier

After collection, records are standardized so that both outlets can be analysed using the same schema. This matters because scraped news data is rarely uniform: field names differ, article bodies may be missing, and duplicate or near-duplicate records can appear in archive exports.

Where relevant, cleaning should be documented in the repository as explicitly as possible, for example:
- removal of duplicate URLs or duplicate article IDs
- exclusion of empty or extremely short texts
- standardization of date fields into a single year variable
- harmonization of source-specific column names
- creation of one merged plain-text field for analysis

### 4. Frame families

The analysis uses four predeclared lexical frame families. These are not full semantic categories in the abstract, but operational dictionaries: grouped terms and phrases that signal recurring ways of talking about immigration.

The four families are:

- **threat_crisis**
- **control_legality**
- **humanitarian**
- **integration_economy**

These families are designed to capture distinct but sometimes overlapping rhetorical logics.

#### threat_crisis

This frame captures language that presents immigration as danger, disruption, pressure, instability, or emergency. It is intended to identify rhetoric that casts migration as a social problem, public threat, or crisis event.

Typical language in this class may include terms such as:
- crisis
- threat
- surge
- wave
- flood
- pressure
- burden
- chaos
- emergency
- invasion

This category is especially useful for identifying alarmist or securitizing rhetoric, but it should be interpreted carefully. Not every use of a word such as “crisis” is necessarily anti-immigration in meaning, which is why contextual validation is essential.

#### control_legality

This frame captures language that emphasizes borders, enforcement, legality, state control, and administrative regulation. It is intended to detect rhetoric that treats migration primarily as a matter of policing, compliance, or legal status.

Typical language in this class may include:
- illegal
- law
- border
- control
- enforcement
- deportation
- detention
- visa
- asylum system
- removal
- smuggling
- trafficking

This frame often overlaps with state and policy discourse. It may be present in articles that are descriptive rather than overtly ideological, which is why it should not automatically be read as hostile without contextual inspection.

#### humanitarian

This frame captures language that foregrounds suffering, protection, vulnerability, refuge, and moral obligation. It is intended to identify rhetoric that presents migrants or refugees in terms of safety, care, rights, and survival.

Typical language in this class may include:
- refugee
- asylum
- protection
- rescue
- humanitarian
- family
- persecution
- war
- violence
- vulnerable
- aid
- shelter

This category helps distinguish articles that frame migration through need, displacement, or human rights concerns rather than through threat or control.

#### integration_economy

This frame captures language focused on contribution, labour, public services, settlement, incorporation, and the socio-economic effects of migration. It is intended to identify discourse about migrants as workers, neighbours, residents, economic actors, or subjects of integration policy.

Typical language in this class may include:
- work
- jobs
- labour
- wages
- economy
- housing
- schools
- NHS
- contribution
- integration
- community
- skills

This frame is useful because it often marks a shift away from explicit crisis rhetoric toward longer-term debates about belonging, capacity, contribution, and incorporation.

### 5. Lexical matching strategy

The project uses lexical matching as a first-pass analytical method. Terms and patterns associated with each frame family are searched within the combined article text, and every match is counted.

This produces several outputs:
- raw hit counts for each frame family
- article-level indicators showing whether a frame appears in a given article
- yearly aggregate totals by outlet
- yearly normalized frequencies per million words
- KWIC outputs for qualitative validation

Lexical matching is deliberately transparent and auditable. Unlike a black-box classifier, it allows the researcher to inspect exactly which words triggered a match and then refine the codebook where necessary.

At the same time, lexical matching is not equivalent to full discourse interpretation. Words do not carry stable meanings in every context, and a term may be quoted, critiqued, negated, or used neutrally. For that reason, keyword tagging should be treated as a discovery tool rather than a final interpretive endpoint.

### 6. Why normalized frequencies are used

Raw counts are useful descriptively, but they are not sufficient for comparison across years or outlets because the volume of available text can vary substantially. Some years contain many more articles and many more words than others. The yearly summary tables therefore report both raw hits and per-million-word rates for each frame family. [file:383][file:385]

The normalization formula is:

`frequency per million = (raw hits / total words) × 1,000,000`

This makes it possible to compare framing intensity across different-sized corpora. A higher normalized value means that the relevant lexical family appears more densely in the text, not simply that more articles were published.

### 7. Why KWIC is necessary

KWIC, or **Key Word in Context**, is exported for every matched pattern so that terms can be inspected in their surrounding textual context. This is a crucial safeguard against over-interpretation.

For example, a word associated with threat framing may appear in:
- a hostile editorial voice
- a quoted politician
- a critical rebuttal
- a neutral report describing another actor’s language

Without contextual checking, these very different uses would all be counted identically. KWIC helps distinguish between them and supports iterative refinement of the dictionary.

### 8. Article-level interpretation

In addition to lexical counts, article-level indicators can be used to identify whether a text contains one or more framing families and, where useful, which frame appears most strongly. These indicators should be treated as heuristic classifications rather than definitive labels.

An article can contain multiple frames at once. For example, a report may combine humanitarian language about refugees with control-oriented discussion of borders or asylum processing. The purpose of the method is therefore not to force texts into a single rigid category, but to detect recurring rhetorical emphases.

### 9. Cleaning and quality control

Because scraped news data is noisy, cleaning is a substantive part of the method rather than a purely technical step. The quality of the final trends depends on how consistently the corpus has been standardized before counting begins.

Recommended cleaning checks include:
- confirming that article dates parse correctly into years
- checking that text fields are not blank after concatenation
- removing duplicate records
- checking for boilerplate or template text that may inflate counts
- validating that counts are generated from article text rather than metadata alone
- checking a sample of articles from each outlet-year for extraction errors

Where possible, the repository should document any exclusions, deduplication rules, or known gaps in coverage so that downstream interpretation remains transparent.

### 10. Interpretation rule

The central interpretation rule for this project is that lexical tagging is a **first-pass discovery method**, not the final claim. The counts identify patterns worth investigating; they do not by themselves prove ideological meaning.

Interpretive claims should therefore be based on a combination of:
- normalized quantitative trends
- KWIC inspection
- article-level reading
- outlet comparison
- historically informed close reading of peak periods

This is especially important when making claims about rhetorical change over time. A rise in a lexical frame indicates increased textual presence of that language family, but the political meaning of that rise should be validated through contextual reading.

### 11. Recommended validation

Before making substantive claims, manually inspect at least several dozen KWIC lines per frame and per outlet-year cluster. This should include both high-frequency years and quieter years, so that the interpretation is not driven only by peaks.

A good validation workflow is:
1. Review high-frequency years for each frame.
2. Read a sample of matched articles in full.
3. Check whether the matched words are being used in the expected framing sense.
4. Refine the dictionary where false positives are common.
5. Re-run the counts after revisions.
6. Record the revision logic in the codebook or README.

This makes the method more robust, more transparent, and easier for other researchers to audit or extend.

### 12. Limits of the method

This approach measures the presence and density of predefined lexical patterns, not the full semantic or ideological content of every article. It is best understood as a structured bridge between distant reading and close reading.

Its strengths are transparency, reproducibility, and interpretability. Its limitations are that it may miss irony, paraphrase, implicit framing, and semantic nuance that does not use the expected keywords.

For that reason, the strongest use of this dataset is not to treat lexical counts as the whole argument, but to use them to identify historical shifts, outlet differences, and periods that merit deeper qualitative analysis.

## Insights

The data shows that immigration coverage is not framed in one stable way across time. Instead, the relative weight of threat/crisis, control/legality, humanitarian, and integration/economy language shifts by year and by outlet, so the graph is best read as a measure of changing rhetorical emphasis rather than simple media attention.

A clear overall pattern is that the Guardian is consistently more humanitarian in its framing, while the Telegraph shows a more mixed profile in which humanitarian language is more often rivalled by control/legality and, in some periods, by integration/economy language. This suggests that the two outlets are not only covering immigration at different intensities, but are also making it legible through different rhetorical priorities.

One of the most important shared shifts appears in the mid-2010s, when threat/crisis language rises sharply in both outlets. Because the graph uses normalised per-million-word frequencies, this rise indicates that crisis language became denser within the coverage itself, not just that more immigration-related articles were published. This makes that period an important rhetorical breakpoint in the dataset.

The later years, especially the 2020s, suggest a stronger normalization of control and legality framing. In the Telegraph in particular, recent coverage gives sustained prominence to the language of borders, enforcement, legality, and administrative control. The Guardian does not mirror this in the same way, but it still shows repeated spikes in threat/crisis language alongside a continued humanitarian emphasis.

Another notable pattern is the relative weakening of the integration/economy frame over time. In both outlets, language focused on contribution, labour, services, and incorporation is comparatively stronger in the mid-2000s and early-to-mid 2010s than it is in much of the later series. This suggests that debates around migration become less centred on settlement and contribution, and more centred on crisis, regulation, and control.

Taken together, the graph points to a broader rhetorical shift in how immigration is discussed in public discourse. Earlier coverage more often clusters around humanitarian need and, at times, social or economic incorporation, while later coverage gives greater space to crisis language and to the vocabulary of management, regulation, and legal control. This does not mean that every article is hostile, but it does suggest that coercive and border-oriented framings become more normalized over time.

This matters for the wider project because it helps show how anti-migrant rhetoric is not only expressed through explicitly extreme language, but also through the normalization of administrative and legal frames. The hostile environment depends not just on law or policy, but on a discourse in which enforcement, exclusion, compliance, and border management come to appear ordinary, practical, and politically common-sense.

At the same time, the persistence of humanitarian language — especially in the Guardian — shows that the rhetorical field remains contested rather than uniformly hardline. The value of the dataset is therefore not that it identifies one single media narrative, but that it maps an ongoing struggle between competing ways of defining immigration: as danger, as a matter of control, as a humanitarian issue, or as a question of social and economic incorporation. This makes the graph most useful as a bridge between distant reading and close reading, helping identify the periods, outlets, and frame shifts that merit deeper historical and qualitative analysis.