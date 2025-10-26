What is the OpenAI Agents SDK? 

OpenAI Agents SDK aik Python package hai jo AI applications banana asaan banata hai. Yeh OpenAI ke purane “Swarm” project ka better aur simple version hai. Ismein kuch main parts hote hain: 

Agents: smart AI units jo soch kar kaam karte hain. 

Handoffs: aik agent se doosray agent tak kaam bhejne ka system. 

Guardrails: safety checks jo sab kuch control mein rakhte hain. 

Yeh sab mil kar complex AI workflows banate hain, aur tracing feature se aap dekh sakte hain ke app ke andar kya ho raha hai. 

Why Use the Agents SDK? 

OpenAI Agents SDK do main goals ke sath bana hai: 

Simple but Powerful: itna simple ke seekhna asaan ho, lekin features itne helpful ho ke kaam asaani se ho jayein. 

Flexible: default setting mein bhi achha kaam karta hai, aur aap apni zarurat ke mutabiq isse customize kar sakte hain. 

Iske khaas features yeh hain: 

Agent Loop: agents ke kaam, sochne ka process aur tool use karne ka system manage karta hai. 

Python-First: Python ke apne functions aur loops ko istimaal karta hai, naye frameworks seekhne ki zarurat nahi. 

Handoffs: aik agent doosray specialist agent ko kaam de sakta hai. 

Guardrails: input aur output check karte hain taake galtiyaan pehle hi pakad li jaayein. 

Function Tools: koi bhi Python function agent ke liye ek tool ban jata hai. 

Tracing: aapko step-by-step process dikhata hai ke agents kya kar rahe hain. 

Running Agents 

Jab aap OpenAI Agents SDK mein kisi agent ko “run” karte hain, to aap asal mein aik AI assistant ko activate kar rahe hote hain jo LLM (Large Language Model) par based hota hai. Yeh agent diye gaye input ko process karta hai aur jawab ya koi kaam perform karta hai. 

Yeh pura process Runner class handle karti hai, jismein teen main methods hoti hain: 

Runner.run(): agent ko asynchronous tareeqe se chalata hai, matlab kaam background mein hota hai. 

Runner.run_sync(): synchronous tareeqe se chalata hai, matlab code tab tak rukta hai jab tak agent apna kaam mukammal nahi kar leta. 

Runner.run_streamed(): real-time updates deta hai jab agent sochta aur jawab banata hai. 

Iska core system agent loop ke zariye kaam karta hai — agent pehle sochta hai, phir koi action leta hai (jaise tool use karna ya task handoff karna), aur yeh process tab tak chalta hai jab tak final output mil jaye ya koi limit tak pohonch jaye. 

Ways to Run an Agent 

The Runner class provides three primary methods to run an agent, each tailored to specific use cases: 

1. Runner.run() 

Type: Asynchronous (async) 
Returns: RunResult object jab agent apna kaam mukammal kar leta hai. 
Use Case: Jab aap chahte hain ke agent background mein kaam kare aur baqi code rukay bina chalta rahe — jaise web apps ya multi-tasking environments mein. 
How It Works: Yeh method agent ko asynchronously start karta hai, jisse code aage chalta rehta hai jab agent input process kar raha hota hai. Jab result ready ho jaye, aap await use karke usay hasil kar sakte hain. 

2. Runner.run_sync() 

Type: Synchronous (sync) 
Returns: RunResult object jab agent ka kaam mukammal ho jata hai. 
Use Case: Aise simple scripts ke liye behtareen jahan aap chahte hain ke agent ka jawab    milne tak code rukay rahe — async handle karne ki zarurat nahi. 
How It Works: Yeh agent ko run karta hai aur execution tab tak block rehta hai jab tak task complete na ho. Andar se yeh Runner.run() ka use karta hai, lekin async logic ko khud manage karke process ko simple bana deta hai. 

3. Runner.run_streamed() 

Type: Asynchronous (async) 
Returns: RunResultStreaming object jo agent ke kaam karte waqt real-time updates deta    hai. 
Use Case: Aise applications ke liye best jahan progress ya partial results dikhane ki zarurat ho — jaise chat interfaces mein streaming text output. 
How It Works: Agent streaming mode mein chalta hai aur task poora hone ka intezar kiye bagair har event (jaise text chunks ya tool updates) turant bhejta rehta hai. 

The Agent Loop: How It Works 

Running an agent ka main hissa agent loop hota hai — yeh aik cycle hai jo Runner manage karta hai aur agent ko final output tak pohonchata hai. Iska step-by-step kaam kuch is tarah hota hai: 

Input Submission: 
Aap agent ko input dete hain, jo simple string ho sakta hai (jaise “What’s the weather today?”) ya structured list (jaise chat history). Yeh input agent ka process start karta hai. 

LLM Invocation: 
Runner yeh input us LLM ko bhejta hai jo agent ke sath linked hota hai. LLM isay agent ke instructions aur tools ke mutabiq process karta hai. 

Output Handling: 
LLM teen type ke outputs de sakta hai: 

Final Output: poora jawab ya result (loop yahan khatam hota hai). 

Tool Calls: kisi tool ko use karne ka request (jaise data fetch karna). 

Handoff: kaam doosray agent ko dene ka faisla. 

Loop Logic: 

Agar Final Output mile: loop band ho jata hai aur result return hota hai. 

Agar Tool Calls hon: runner tool run karta hai, results add karta hai, aur loop dobara chalata hai. 

Agar Handoff ho: current agent se naya agent switch hota hai, aur process dobara shuru hota hai. 

Termination: 
Yeh loop tab tak chalta rehta hai jab tak final output na mil jaye, ya maximum turns limit (max_turns) cross na ho jaye — agar limit exceed ho jaye to MaxTurnsExceeded exception throw hoti hai taake infinite loop na banay. 

Key Mechanics 

Final Output Definition: Output “final” tab hota hai jab LLM directly text ya structured response de (agar koi optional output_type diya gaya ho to uske mutabiq) aur koi tool call na kare. 

Tools: Yeh aisi functions hote hain jinhein agent use kar sakta hai — jaise APIs call karna ya calculations karna. In tools ke results dobara loop mein bheje jaate hain taake agent unhe process karke kaam aage barhaye. 

Handoffs: Yeh feature agent ko allow karta hai ke wo task kisi specialized agent ko transfer kare. Is se loop ke dauran hi active agent smoothly switch ho jata hai bina process rukay. 

Customizing Runs with run_config 

Aap run_config parameter use karke agent run ke global settings customize kar sakte hain. Mashhoor options yeh hain: 

model: Sab agents ke liye ek specific LLM model set karta hai, unke default ko override karte hue. 

model_provider: Model names ko unke actual implementations se map karta hai. 

input_guardrails aur output_guardrails: Input aur output par safety aur quality checks enforce karte hain. 

tracing_disabled: Tracing ko band karta hai (default mein debugging ke liye enabled hoti hai). 

workflow_name: Run ko ek naam deta hai taake dashboard mein tracking asaan ho. 

Yeh settings aapko agent ke behavior aur monitoring par poora control deti hain. 

Multi-Turn Conversations 

Har run method ka call aik “turn” ko represent karta hai conversation mein: 

First Turn: User aik message deta hai, aur agent uska jawab deta hai. 

Next Turn: Pehle wale output ko RunResult.to_input_list() ke zariye input list mein badalte hain, phir user ka naya message add karke agent ko dobara run karte hain. 

Yeh process context ko har turn mein maintain karta hai, jisse interactive chats aur workflows possible hote hain. 

Potential Exceptions 

Agent run karte waqt kuch errors (exceptions) aa sakte hain jo problems ko signal karte hain: 

MaxTurnsExceeded: Loop bohot dafa repeat hua — apni max_turns limit check karein. 

ModelBehaviorError: LLM ne invalid output diya (jaise galat JSON ya fake tool reference). 

InputGuardrailTripwireTriggered / OutputGuardrailTripwireTriggered: Guardrail check fail hua, run ruk gaya. 

UserError: SDK ka galat istemal (jaise parameters ka issue). 

In errors ko handle karna zaroori hai taake application stable aur reliable rahe. 

OpenAI Agents SDK mein agent run karna matlab hai aik AI assistant launch karna jo input ko LLM reasoning, tools aur agent handoffs ke dynamic loop se process karta hai. Runner.run(), Runner.run_sync(), aur Runner.run_streamed() ka use karke aap apni zarurat ke mutabiq asynchronous, synchronous, ya streaming mode select kar sakte hain. Yeh process agent loop ke zariye chalti hai, jise run_config se customize kar sakte hain, aur multi-turn conversations ke liye support bhi deti hai. 

Streaming 

What is Streaming? 

Streaming ka matlab hai ke agent kaam karte waqt hi updates milti rehti hain, poora task khatam hone ka intezar nahi karna padta. Yeh bilkul aise hai jaise aap live video dekh rahe ho bajaye poori movie download karne ke. 

OpenAI Agents SDK mein streaming se aapko real-time partial results, tool calls, aur events milte hain jab agent process kar raha hota hai. Iska faida yeh hai: 

User Experience: Real-time progress dikhata hai, jaise chatbot mein text type hota hua nazar aaye. 

Efficiency: Lambi tasks ke dauran app block nahi hoti. 

Debugging: Step-by-step agent ke actions dekhne ki suvidha milti hai. 

How Does Streaming Work? 

Streaming use karne ke liye aap Runner.run_streamed() method call karte hain, jo RunResultStreaming object return karta hai. Yeh object apne result.stream_events() method ke zariye asynchronous stream of events deta hai. Har event aik StreamEvent hota hai, aur yeh mukhtalif types mein aate hain: 

Raw Response Events: LLM (language model) ke low-level updates jaise chhote text chunks (text deltas) jab woh generate ho rahe hote hain. 

Run Item Events: High-level updates jaise jab koi tool call hota hai, complete hota hai, ya message mukammal ho jata hai. 

Agent Update Events: Jab active agent change hota hai — jaise handoff ke dauran — to notification milta hai. 

Aap inn events ko asynchronously iterate kar sakte hain aur har event ke aane par response de sakte hain. 

Why Use Streaming? 

Real-Time Feedback: Users ko turant progress dikhakar unhe engaged rakhta hai. 

Non-Blocking: App ko allow karta hai ke agent ke kaam karte waqt bhi doosre tasks chalte rahein. 

Visibility: Agent ke real-time actions dikhakar debugging asaan banata hai. 

Tracing 

What is Tracing? 

Tracing OpenAI Agents SDK ka aik built-in system hai jo har agent run ke dauran hone wale tamam important actions ko record karta hai. Yeh ek tarah ka flight recorder hota hai jo detailed activity log maintain karta hai, jaise LLM calls, tool executions, guardrail checks, aur agent handoffs. 

Tracing se SDK poore workflow ko two main parts mein organize karta hai: 

Traces: Yeh poore workflow ka record hoti hain, jisme unique ID, workflow name, aur optional metadata hota hai. 

Spans: Yeh trace ke andhar chhoti operations hoti hain — jaise LLM generation, tool call, ya handoff. 

Tracing default taur par enabled hoti hai, lekin agar chahein to aap environment variable (OPENAI_AGENTS_DISABLE_TRACING=1) ya run configuration (RunConfig.tracing_disabled=True) ke zariye ise disable kar sakte hain. 

Iska faida yeh hai ke aap apne agents ke workflow ko debug, optimize, aur monitor kar sakte hain. Tracing se aapko performance aur behavior ke bare mein precise insights milte hain, aur sab kuch OpenAI Dashboard par visually dekhne ko milta hai. 

How Does Tracing Work? 

Tracing OpenAI Agents SDK ke do main concepts par based hoti hai: Traces aur Spans. 

Traces: Yeh aik complete operation ka record hoti hain, jaise aik poora agent run. Har trace ke andar aik unique trace_id, ek optional group_id (conversation ya workflow ke related traces link karne ke liye), aur metadata hoti hai jo workflow ka naam aur extra details store karti hai. 

Spans: Yeh trace ke chhote parts hote hain jo specific actions represent karte hain — jaise LLM generation, tool call, ya guardrail check. Har span mein: 

Timestamps: started_at aur ended_at hotay hain jo show karte hain ke action kab start aur end hua. 

Data: Ismein inputs, outputs, ya kisi bhi relevant information ka detail hota hai, jaise GenerationSpanData (LLM ke liye) ya FunctionSpanData (tool ke liye). 

In dono concepts ke through, tracing agent ke workflow ka poora structure dikhata hai, jisse debugging, optimization, aur monitoring asaan hoti hai. 

Default Tracing Behavior 

Tracing OpenAI Agents SDK mein default taur par enabled hoti hai aur agent ke workflow ke tamam important processes ko capture karti hai. SDK in tamam activities ko alag-alag spans ke zariye track karta hai, jinhein ek single trace ke andar organize kiya jata hai. 

Default tracing system is tarah kaam karta hai: 

Runner.run() / Runner.run_streamed(): Pooray run ko ek single trace ke roop mein record karta hai. 

AgentSpan: Har baar jab agent invoke hota hai, ek span create hoti hai. 

GenerationSpan: Har LLM generation (model output) record karti hai. 

FunctionSpan: Jab koi tool call hota hai to uska execution is span mein log hota hai. 

GuardrailSpan: Input ya output guardrail checks record karti hai. 

HandoffSpan: Jab agent doosray agent ko kaam handoff karta hai, yeh span woh activity capture karti hai. 

TranscriptionSpan (speech‑to‑text) aur SpeechSpan (text‑to‑speech): Voice-based agents ke liye ye spans audio processing record karti hain. 

Sabhi spans aur traces automatically OpenAI ke backend par bheje jaate hain aur OpenAI Dashboard mein visual format mein dekhe ja sakte hain, jahan developers easily debugging, monitoring, aur optimization kar sakte hain. 

Customizing Traces 

OpenAI Agents SDK mein tracing ko aap apni zarurat ke mutabiq tweak kar sakte hain. Yeh kuch tareeqe hain: 

Disable Tracing: 
Agar aapko tracing band karni ho, to do options hain: 

Environment variable set karein — OPENAI_AGENTS_DISABLE_TRACING=1 

Ya phir RunConfig mein tracing_disabled=True pass karein. Yeh sirf uss run ke liye tracing ko disable karega. 

Name Your Workflow: 
Agar aap chahte hain ke aapka trace meaningful naam ke sath dashboard par dikhe, to: 

RunConfig mein workflow_name set karein, ya 

Aap apna code with trace("My Workflow") ke block se wrap kar sakte hain taake related operations ek hi workflow ke under group hoon. 

Add Custom Spans: 
Agar aap apne code ka koi specific hissa trace karna chahte hain, to custom_span() function use karein. Yeh custom timestamps aur detailed data ke sath span create karta hai, jise aap debugging aur analysis ke liye use kar sakte hain. 

In customization options ki madad se aap tracing ko poora control kar sakte hain — chahe development ke liye debugging ho, ya production mein performance tracking. 

Handling Sensitive Data 

Tracing OpenAI Agents SDK mein default taur par inputs aur outputs ko include karta hai, jo kabhi kabhi sensitive information bhi contain kar sakte hain. Agar aap apni application mein privacy ya security maintain rakhna chahein, to tracing ko customize karke sensitive data ko exclude kar sakte hain. 

Iske liye simple configuration hoti hai — aap RunConfig mein tracing ke liye ek setting enable karte hain jismein trace_include_sensitive_data ko false set kiya jata hai. Isse tracing inputs aur outputs ko record nahi karti. 

Yeh option un cases mein bohot zaroori hoti hai jahan user prompts, confidential text, ya private business data trace logs mein show na ho. Agar aur bhi strict privacy control chahiye ho, to environment level settings se LLM aur tool data logging ko disable kiya ja sakta hai. 

Is tarah tracing useful debugging aur monitoring ke liye active rehti hai, lekin sensitive user data safe aur private rehta hai. 

Custom Trace Processors 

OpenAI Agents SDK mein tracing system flexible hai, aur aap chahein to apne traces ko OpenAI ke default dashboard ke ilawa kisi aur jagah — jaise apne logging system, monitoring tool, ya third-party observability platform — par bhej sakte hain. 

Iske liye do options diye gaye hain: 

Add a Processor: 
Is option ke zariye aap ek naya trace processor add kar sakte hain jo traces ko additional destination par send karega. Default system OpenAI ke backend ko traces bhejta rahega, jabke aapka added processor unhe extra jagah par bhi push karega. 

Replace Processors: 
Agar aap chahte hain ke traces sirf apke custom system par hi bheje jayein (aur OpenAI dashboard par na jaayein), to aap existing processors ko replace kar sakte hain. Yeh approach aapko poora control deta hai ke traces kahan aur kaise store ya monitor kiye ja rahe hain. 

In methods ki help se aap tracing workflow ko apne infrastructure ke mutabiq customize kar sakte hain — chahe aap apni private analytics setup use kar rahe ho, ya external services jaise Datadog, Phoenix, ya Langfuse. 

Why Use Tracing? 

Tracing OpenAI Agents SDK ka ek important feature hai jo agent ke workflow ke har step ko detail mein record karta hai. Yeh system development aur production dono environments mein debugging, performance improvement, aur compliance ke liye valuable hota hai. 

Debugging: Tracing se aap poore workflow ka exact sequence dekh sakte hain — kis order mein LLM calls, tool executions, aur handoffs huye. Isse errors aur unexpected behaviors diagnose karna bohot asaan ho jata hai. 

Performance: Trace data se aap identify kar sakte hain ke kaunsa step workflow mein sabse zyada time le raha hai, aur bottlenecks easily locate kar sakte hain. Yeh optimization ke liye useful hota hai. 

Auditing: Har agent action aur user interaction ka accurate record maintain hota hai, jo compliance, transparency, aur operational analysis ke liye important hai. 

In sab ka visualization OpenAI dashboard mein available hota hai, jahan se developers trace events ko dekh, filter, aur analyze kar sakte hain taake system aur workflows dono ko behtar banaya ja sake. 

Conversational Agents 

What Are Conversational Agents? 

Conversational agents aise AI-powered systems hain jo insano ke saath natural aur human-like dialogue karne ke liye design kiye gaye hain. Yeh large language models par based hote hain — jaise OpenAI ke GPT models — jinmein natural language samajhne aur generate karne ki ability hoti hai. 

In agents ka kaam sirf sawalon ke jawab dena nahi, balki context samajhna, multi-turn conversations maintain karna, aur actions perform karna bhi hota hai. Yeh unhe simple chatbots se zyada smart aur adaptive banata hai. 

Unke kuch core features yeh hain: 

Understanding User Input: Yeh casually ya indirectly puche gaye questions ka meaning samajh lete hain, chahe language vague ho. 

Generating Responses: Har response contextually relevant aur coherent hota hai, jo human-style conversation ko mimic karta hai. 

Maintaining Context: Yeh pehle ke messages aur interactions yaad rakhte hain taake conversation flow natural aur connected lage. 

Performing Actions: Sirf baat cheet tak limited nahi, yeh tools use karke tasks execute kar sakte hain, jaise data fetch karna, reminders lagana, ya kisī aur agent ko handoff dena. 

Inhe aap advanced virtual assistants samajh sakte hain — Siri ya Alexa jaise — lekin zyada flexible aur smart, jo complex queries handle karne aur personalized experience dene mein capable hote hain. 

How Do They Work in the OpenAI Agents SDK? 

OpenAI Agents SDK mein conversational agent banane ke liye Agent class ka use hota hai. Yeh class AI ko ek specific role aur behavior dene ke liye design ki gayi hai, jisse wo natural conversation ke zariye user ke input ko samajh kar jawab de sakta hai. 

Aap jab koi agent define karte hain, to teen main elements specify kiye jaate hain: 

Instructions: Yeh agent ka base prompt hota hai jo uska role aur tone set karta hai, jaise “You are a friendly travel advisor.” 

Tools: Yeh woh functions hain jinke zariye agent real-world actions perform karta hai, jaise APIs call karna ya calculations karna. 

Handoffs: Agar koi kaam specialized agent ke liye better hai, to current agent woh task usko handoff kar deta hai. 

Agent ka interaction Runner class ke zariye hota hai, jo conversation ko ek loop ke form mein manage karta hai: 

User apna input bhejta hai, jaise “What’s the weather like today?”. 

Agent input ko LLM ke zariye analyze karta hai aur intent samajhta hai. 

Phir yeh kisi teen routes mein se ek choose karta hai: 

Direct jawab dena, 

Kisi tool ka use karke information fetch karna, 

Ya task ko dusre agent ko delegate kar dena. 

Yeh loop tab tak chalta hai jab tak user ka poora request complete na ho jaye. 

Yeh design conversational agents ko powerful banata hai — jo not only baat karte hain, balki intelligent actions le kar problems solve karne ki capability bhi rakhte hain. 

Multi-Turn Conversations 

Conversational agents multi-turn interactions mein tabhi consistent rahte hain jab wo pehle ke messages ka context yaad rakhte hain. OpenAI Agents SDK is process ko do tareeqon se handle karta hai: manual context preservation aur built-in sessions. 

Manual tareeqe mein agent ka output ek list ke form mein milta hai, jise phir next turn ke input ke sath merge kiya jata hai. Yeh RunResult.to_input_list() ke zariye hota hai — is method se agent conversation ka pura history retrieve karta hai. Aap ismein agla user message add karte hain aur updated list dobara agent ko bhejte hain. Isse agent har naye response mein ongoing conversation ka context maintain karta hai. 

Lekin agar aapko har turn par manual tracking nahi chahiye, to SDK ka Session system ye kaam automatically karta hai. Session ek ID ke sath linked hota hai aur us conversation ki puri history store karta hai. Jab bhi agent ko wo session pass kiya jata hai, wo pehle ke sab interactions ko automatically recall kar leta hai. 

Dono approaches ka goal ek hi hai — context ko preserve karke naturally flowing dialogue maintain karna, jahan agent har naye jawab mein pehle ke conversations ko samajh kar coherent aur human-like responses de sake. 

Why Are Conversational Agents Useful? 

Conversational agents users ke liye interaction ko asaan aur natural banate hain, jahan unhe technology seekhne ki zarurat nahi hoti. Yeh systems human-style dialogue ke zariye communication ko intuitive aur efficient banate hain. 

Unki kuch khaas capabilities yeh hain: 

Natural Interaction: Users apni rozmarra ki zubaan mein baat kar sakte hain, chahe phrasing simple, casual, ya imperfect ho. Yeh learning curve ko kam karta hai aur technology sabke liye accessible banata hai. 

Context Awareness: Agents conversation ka flow maintain karte hain — wo pehle ke sawalon aur jawab yaad rakhte hain taake responses repetitive na lagen. Iss se dialogue coherent aur connected rehta hai. 

Task Versatility: Yeh sirf queries ka jawab dene tak limited nahi hote; balki tasks complete karte hain jaise information fetch karna, bookings karna, reports banana, ya doosre agents ko kaam handoff karna. 

Inn features ke zariye conversational agents ek smart, context-aware assistant ki tarah kaam karte hain jo users ke natural communication pattern ko samajh kar unhe personalized aur efficient responses deta hai. 

Function Tools 

What Are Function Tools? 

Function tools OpenAI Agents SDK ka integral part hain jo conversational agents ki capabilities ko text generation se aage badhate hain. Yeh agents ko real actions perform karne ki ability dete hain — jaise data retrieve karna, calculations chalana, ya kisi external API ke sath interact karna. 

In tools ko define karne ke liye SDK mein ek asaan mechanism diya gaya hai. Aap koi bhi Python function ko tool bana sakte hain, aur system uske liye automatic setup kar deta hai. 

Function tools ke kuch core features yeh hain: 

Automatic Identification: Har function ka naam aur docstring se uska naam aur description assign hota hai, jisse agent us function ke purpose ko samajh leta hai. 

Input Schema Generation: Function ke parameters ke base par automatic input structure ban jata hai, taake agent sahi format mein data bhej sake. 

Validation and Error Handling: Input values validate hoti hain aur agar koi galat ya missing parameter ho, to SDK graceful error handling karta hai. 

Yeh tools agent ke workflow mein seamlessly integrate hote hain. Jab agent kisi user request ka analysis karta hai aur samajhta hai ke response generate karne ke bajaye action chahiye — jaise weather check karna ya data fetch karna — to wo directly related function tool call kar deta hai. 

Is approach se agents sirf baat karne wale bots nahi rehte, balki intelligent assistants ban jaate hain jo soch kar kaam karte hain aur practical results deliver karte hain. 

How Do Function Tools Work? 

OpenAI Agents SDK mein jab agent ko koi kaam karna hota hai, to wo apne workflow ke andar automatically sahi tool identify karta hai aur use execute karne ke liye zaruri arguments deta hai. Yeh puri process SDK ke agent loop mein smoothly integrate hoti hai, jisme agent text generation ke saath saath real tasks bhi perform kar sakta hai. 

Process kuch is tarah hoti hai: 

Pehle agent user ke input ko analyze karta hai aur samajhta hai ke action lena zaruri hai ya sirf jawab dena. 

Agar action required ho, to wo relevant function tool choose karta hai aur uske liye arguments generate karta hai. 

SDK background mein us function ko run karta hai aur uska result wapas agent ko deta hai. 

Agent us output ka use karke apna next step tay karta hai — chahe wo final jawab banana ho, doosra tool call karna ho, ya kisi aur agent ko handoff dena ho. 

Yeh process agent ke loop ka integral part hoti hai, isliye developer ko manually tool management karne ki zarurat nahi padti. Tool usage completely intuitive aur automated hoti hai, jisse agent natural conversation flow ke andar data retrieval, computation, ya system interaction jaise tasks seamlessly handle karta hai. 

Customizing Function Tools 

OpenAI Agents SDK mein FunctionTool manually define karne ka option un scenarios ke liye diya gaya hai jahan built-in decorators kaafi flexible nahi hote — jaise complex APIs ya databases se connect karna. 

Manual definition agent developers ko complete control deti hai tool ke behavior aur data structure par. Aap yahan apna tool step-by-step customize kar sakte hain: 

Name: Har manually defined tool ko ek unique naam diya jata hai taake agent usse identify kar sake. 

Description: Yeh tool ke purpose ka short explanation hota hai, jo LLM ko samajhne mein madad karta hai ke tool kab aur kyun use karna hai. 

Params Schema: Input parameters ke liye ek structured JSON schema define kiya jata hai, jisse arguments ki validation aur type-checking automatically ho jati hai. 

On Invoke: Yeh ek asynchronous function hota hai jo tab execute hota hai jab agent tool call karta hai. Yeh function input process karke output return karta hai. 

Is approach ka faida yeh hai ke aap SDK ko apne custom business logic ke sath integrate kar sakte hain — jaise internal databases, REST APIs, ya cloud systems. FunctionTool agent ke loop mein seamlessly fit hota hai, jisse conversational flow disturb hue bina external data ya computation kaam perform kiya ja sakta hai. 

Agents as Tools 

OpenAI Agents SDK mein aap ek agent ko tool mein convert kar sakte hain, jisse doosre agents usse directly call karke madad le sakte hain — bina conversation ka full control handoff kiye. Is feature ko “Agents-as-Tools” kehte hain. 

Yeh approach multi-agent coordination ke liye useful hoti hai, jahan ek primary agent kisi specialized agent ko temporary taur par use karta hai kisi specific task ke liye — jaise coding, data analysis, ya translation. 

Iska kaam karne ka tareeqa kuch is tarah hota hai: 

Aap ek agent define karte hain (jaise ek expert writer ya data retriever). 

Phir us agent ko as_tool function ke zariye ek callable tool ke roop mein expose kar dete hain. 

Jab dusra agent apne workflow ke dauran us specialized task ka samna karta hai, to wo directly us “agent-tool” ko invoke karta hai. 

Us agent-tool ka output wapas us agent ko milta hai jisse call kiya gaya tha, jisse wo apna conversation ya kaam seamlessly continue kar pata hai. 

Yeh design modular AI systems ke liye bohot powerful hai, kyunke isse alag-alag agents apni expertise maintain karte hue cooperative way mein kaam kar sakte hain — bina user ke perspective se conversation flow break kiye. 

Why Are Function Tools Useful? 

OpenAI Agents SDK agents ko sirf conversational systems nahi, balki action-oriented AI banata hai — aise systems jo real-world tasks execute kar sakte hain. Yeh design developers ke liye smart, reusable aur scalable agents banana asaan banata hai. 

Is SDK ki kuch key qualities yeh hain: 

Action-Oriented: Agents text responses ke ilawa real-world ke sath interact kar sakte hain — jaise APIs call karna, data fetch karna, ya processes automate karna. Isse wo sirf answer dene wale bots nahi, balki decision-making systems ban jaate hain jo objectives achieve karte hain. 

Reusable: Tools aur agents modular design mein bante hain. Ek hi tool multiple agents ke beech share kiya ja sakta hai, jisse duplication kam hota hai aur ecosystem ek coordinated system ki tarah kaam karta hai. 

Simplified Development: SDK developer ke liye complexities abstract karta hai. Aapko advanced orchestration ya manual state management handle nahi karna padta — bas function likhna hota hai, aur SDK unhe automatic tools mein convert kar deta hai. 

Yeh balance of power aur simplicity SDK ko un use cases ke liye ideal banata hai jahan AI ko reasoning, planning aur execution — tino capabilities ek sath chahiye hoti hain. 

 