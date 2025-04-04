<template>
  <div class="speaker-container">
    <!-- 左侧导航栏 -->
    <div class="sidebar">
      <h3 class="sidebar-title">Speakers</h3>
      <ul class="speaker-list">
        <li v-for="(speaker, index) in speakers" :key="index" :class="{ active: selectedSpeaker === speaker }"
          @click="selectedSpeaker = speaker">
          {{ speaker.name }}
        </li>
      </ul>
    </div>

    <!-- 右侧内容区域 -->
    <div class="speaker-details">
      <div v-if="selectedSpeaker" class="details-content">
        <h2 class="speaker-name">{{ selectedSpeaker.name }}</h2>


        <div class="talk-title" v-if="selectedSpeaker.talkTitle">
          <h3>{{ selectedSpeaker.talkTitle }}</h3>
        </div>
        <div class="speaker-info">
          <div class="speaker-image-container">
            <img :src="selectedSpeaker.image" :alt="selectedSpeaker.name" class="speaker-image" />
          </div>
          <div class="speaker-bio">
            <p v-for="(paragraph, idx) in selectedSpeaker.bio" :key="idx">
              {{ paragraph }}
            </p>
          </div>
        </div>
        <div class="speaker-contact">
          <div class="institutions">
            <strong>Institution{{ selectedSpeaker.institutions.length > 1 ? 's' : '' }}:</strong>
            <ul class="institution-list">
              <li v-for="(institution, idx) in selectedSpeaker.institutions" :key="idx">
                {{ institution }}
              </li>
            </ul>
          </div>
        </div>
      </div>
      <div v-else class="select-prompt">请从左侧选择一位组织者查看详细信息</div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref, watch } from 'vue'
import { useRoute } from 'vue-router';
const baseUrl = import.meta.env.BASE_URL;
const speakers = [
  {
    name: 'Amy Arnsten',
    image: `${baseUrl}AmyArnsten.jpg`,
    institutions:
      [
        'Albert E. Kent Professor of Neuroscience and Professor of Psychology',
        'Yale School of Medicine',
        'Member of Kavli Institute of Neuroscience at Yale University'
      ],
    bio:
      [
        'Arnsten received her BA in Neuroscience with Honors from Brown University in 1976 and her PhD in Neuroscience from UCSD in 1981, with postdocs at Cambridge with Susan Iversen and then Yale with Patricia Goldman-Rakic. She became Assistant Professor at Yale in 1986 and is currently the Albert E. Kent Professor of Neuroscience. She is a member of the National Academy of Medicine and received the Goldman-Rakic Prize for Outstanding Research in Cognitive Neuroscience. Arnsten’s research has led to the development of guanfacine (Intuniv™) for the treatment of ADHD and other prefrontal disorders, including recent studies treating the cognitive deficits of long-COVID and delirium and prazosin for the treatment of post-traumatic stress disorder.'
      ],
    talkTitle: 'Stress and fatigue impair prefrontal cortical function needed for perseverance and higher cognition'
  },
  {
    name: 'Haruhiko Bito',
    image: `${baseUrl}HaruhikoBito.jpg`,
    institutions:
      [
        'Professor and Chair, Department of Neurochemistry',
        'Chair, Division of Neuroscience',
        'Director, Center for Disease Biology and Integrative Medicine',
        'Graduate School of Medicine, The University of Tokyo'
      ],
    bio:
      [
        'Haruhiko Bito is a Professor and Chair of Neurochemistry at the University of Tokyo. He graduated from the University of Tokyo with an M.D. and a Ph.D. in Biochemistry in 1993. After postdoctoral training at Stanford University as an HFSP Fellow, he started his laboratory in Pharmacology at Kyoto University in 1997 before moving to head the Department of Neurochemistry at the University of Tokyo in 2003. Dr. Bito has deciphered novel functions of many members of the CaMK family and elucidated the bidirectional neuronal signaling between the synapses and the nucleus, essential for late-phase plasticity and long-term memory. Dr. Bito has also designed powerful molecular tools (E-SARE synthetic activity-dependent enhancer and next-generation Ca2+ indicators XCaMPs) that help capture and measure neuronal ensemble activity critical for cognition. He received the Young Investigator Awards from the Japanese Societies for Pharmacology (2003) and Biochemistry (2004), the Tsukahara Award from the Japan Neuroscience Society (2011), AND Investigator Award (Molecular Brain, 2015), and the Setsuro Ebashi Award (2020). '
      ],
    talkTitle: 'Towards understanding complex neural dynamics via multiplex imaging of neuronal activity and signaling'
  },

  {
    name: 'Yue Li',
    image: `${baseUrl}yueli.jpg`,
    institutions: [
      'State Key Laboratory of Medical Neurobiology',
      'MOE Frontiers Center for Brain Science',
      'Institutes of Brain Science, Shanghai Medical College, Fudan University',
    ],
    bio: [
      'Li Yue, graduated with a PhD from the School of Basic Medicine at Zhejiang University in 2017, and conducted postdoctoral research at the Medical School of the University of Geneva in Switzerland from 2018 to 2022. Mainly engaged in research on drug addiction and synaptic plasticity, with the main results published as the first author in Science, Renowned journals such as Nature and Neuron. Joined the Institute of Neuroscience Center for Frontiers in Neuroscience at Fudan University as the project leader in December 2022. Selected for the National and Shanghai High level Youth Talent Program.',
    ],
    talkTitle: 'he Modulation of Persevering Drug Administration by Serotonin'
  },
  {
    name: 'Jifan Shi',
    image: `${baseUrl}JifanShi.png`,
    institutions: ['Research Institute of Intelligent Complex Systems,Fudan University'],
    bio: [
      "Ji-Fan Shi is a Junior Associate Researcher at the Research Institute of Intelligent Complex Systems. He obtained both his Bachelor's and Ph.D. degrees in Computational Mathematics from the School of Mathematical Sciences at Peking University. Prior to joining Fudan University in May 2022, he held positions at the Institute of Industrial Science and the International Research Center for Neurointelligence at the University of Tokyo. In May 2021, he was selected for the Shanghai Leading (Overseas) Young Talent Program, and in 2023, he was included in the National Young Talent Program. His primary research interests lie in computational biology and the mathematical theory of complex life systems, with a focus on interdisciplinary topics such as stochastic models and algorithms, biological system modeling and analysis, complex network inference, and critical phenomenon analysis.",
    ],
  },
  {
    name: 'Rafał Ryguła',
    image: `${baseUrl}RafałRyguła .jpg`,
    institutions:
      [
        'Head of the Affective Cognitive Neuroscience Laboratory',
        'Maj Institute of Pharmacology Polish Academy of Sciences'
      ],
    bio:
      [
        'A graduate of the Faculty of Biology and Earth Sciences at Jagiellonian University. In 2006 he defended his doctoral thesis at the University of Göttingen in Germany. From 2008 to 2011, he conducted research at the Department of Experimental Psychology at the University of Cambridge, UK. Since 2011, he has been associated with the Jerzy Maj Institute of Pharmacology of the Polish Academy of Sciences, where he leads the Affective Cognitive Neuroscience Laboratory. His scientific interests include the interactions between emotions and cognition, the cognitive mechanisms of mental disorders, addictions, and, more recently, susceptibility to (dis)information.'
      ],
    talkTitle: 'Modeling Reinforcement-Based Cognitive Biases in Animal Models of Mental Disorders and Addictions.'
  },
  {
    name: 'Yang Yang',
    image: `${baseUrl}YangYang.jpg`,
    institutions:
      [
        'School of Life Science and Technology, ShanghaiTech University',
        'State Key Laboratory of Advanced Medical Materials and Devices'
      ],
    bio:
      [
        "Dr. Yang Yang, graduated with a bachelor's degree in Biomedical Engineering from Zhejiang University, and a Ph.D.degree in Neuroscience from Stony Brook University.She joined the School of Life Sciences at ShanghaiTech University in 2017. Her laboratory uses cutting - edge techniques including in vivo two - photon imaging, in vivo multi - channel electrophysiology and optogenetics in behaving animals, to investigate neural circuits and synaptic plasticity mechanisms underlying cognitive behaviors such as decision making and learning.She has published a series of papers in Nature Neuroscience, Journal of Neuroscience, National Science Review, and Cell Reports.She receives funding from the National Natural Science Foundation of China, the National Key R & D Program of the Ministry of Science and Technology, and \"Science and Technology Innovation 2030\" of the Ministry of Science and Technology of China."
      ],
    talkTitle: 'Posterior parietal cortex mediates rarity-induced decision bias and learning under uncertainty'
  },
  {
    name: 'Nashat Abumaria',
    image: `${baseUrl}NashatAbumaria.jpg`,
    institutions: [
      'State Key Laboratory of Medical Neurobiology',
      'MOE Frontiers Center for Brain Science',
      'Institutes of Brain Science, Fudan University',
    ],
    bio: [
      "Nashat Abumaria has been a professor and principal investigator at the State Key Laboratory of Medical Neurobiology, MOE Frontiers Center for Brain Science and Institutes of Brain Sciences, Fudan University, Shanghai, China, since 2015. He obtained his PhD from the University of Göttingen in Germany within the International Max-Planck Research Program for Neuroscience (IMPRS). The Program Dean is Nobel laureate Erwin Neher. The prestigious Christoph-Lichtenberg fellowship by the Government of Lower-Saxony supported Dr. Abumaria. He did a short postdoc fellowship in Prof. Eberhard Fuchs’ Lab (Winner of the German presidential award for lifetime achievement) at the German Primate Center, Goettingen, Germany. In 2007, he moved to China and did a postdoc fellowship in Prof. Liu Guosong’s Lab at the School of Medicine, Tsinghua University, Beijing, China. In 2011, he was appointed as a research associate professor in the Department of Basic Medical Science, School of Medicine, Tsinghua University. His work at Tsinghua University focused on mechanisms of enhancing synaptic plasticity and cognitive functions, including fear memories (In collaboration with the noble laureate Susumu Tonegawa’s Lab at MIT and Min Zhou’s lab at Toronto University). Dr. Abumaria has published numerous peer-reviewed scientific papers. He has been awarded several research grants, including China postdoctoral research funding, the National Natural Science Foundation of China (NSFC) grant for young investigators, as well as several other NSFC grants, 973 Science & Technology, and 985 talent recruitment grants. In 2024, he received the prestigious “China International Senior Scientists fund”. The fund is considered the highest research fund that a foreign scholar can obtain at the individual level from NSFC. He serves as editor, associated editor and reviewer for several scientific journals. "
    ],
    talkTitle: 'Behavioral switch from action to no action when facing prolonged uncontrollability or repeated failure'
  },
  {
    name: 'Misha Benjamin Ahrens',
    image: `${baseUrl}MishaBenjaminAhrens.jpg`,
    institutions:
      [
        'Research group leader at Janelia Research Campus, HHMI'
      ],
    bio:
      [
        "Misha Ahrens studied mathematics and physics at Cambridge University and did his PhD in computational neuroscience at the Gatsby Unit, UCL. He then moved to Harvard University to do a postdoc with Florian Engert in experimental neuroscience, after which he became a Group Leader at HHMI Janelia Research Campus in 2012. He is now a Senior Group Leader at Janelia focusing on systems neuroscience, brain-body interactions, and neurotechnology."
      ],
    talkTitle: 'hole-brain and whole-body computations for behavior and physiology'
  },
  {
    name: 'Wenzhi Sun',
    image: `${baseUrl}WenzhiSun.jpg`,
    institutions:
      [
        'Chinese Institute for Brain Research, Beijing',
        'Capital Medical University College of Basic Sciences'
      ],
    bio:
      [
        "Dr. Sun Wenzhi obtained his degree from the Institute of Neuroscience at the Chinese Academy of Sciences and subsequently engaged in academic and research activities at three esteemed research institutions. From 2004 to 2010, he pursued postdoctoral research at the University of California, Berkeley, under the guidance of Professor Yan Dan, a distinguished member of the National Academy of Sciences and a prominent researcher at the Howard Hughes Medical Institute. Thereafter, he held the position of senior staff scientist at The Jackson Laboratory in the United States from 2010 to 2012, during which he conducted systematic investigations in mouse genetics. From 2012 to 2016, he was affiliated with the Janelia Research Campus of the Howard Hughes Medical Institute, where he concentrated on the development and application of optical imaging technologies. In September 2016, he returned to China to establish a laboratory at the iHuman Institute, ShanghaiTech University, which integrates advanced neuroscience research methodologies, including gene engineering, optogenetics, in vivo electrophysiology, and optical imaging. In December 2018, his laboratory became associated with the Beijing Institute of Brain Science and Intelligence Research.Dr. Sun Wenzhi's laboratory focuses on two primary research domains: (1) the neural circuit mechanisms underlying emotions, and (2) the advancement of novel cell-targeted gene delivery technologies and DNA therapeutics. By employing neural circuitry and in vivo optical microscopy imaging techniques, the laboratory has produced a series of innovative findings. It has published over 40 articles in high-impact journals, including Nature, Neuron, Nature Neuroscience, Nature Communications, Current Biology, PNAS, Cell Reports, Science Advances, and Molecular Therapy, in addition to filing for 12 instrument patents and 4 PCT invention patents."
      ],
    talkTitle: 'Chronic Stress Reinforces the BLA-mPFC-LHb Neural Circuit Initiating Depression'
  },
  {
    name: 'Jingfeng Zhou',
    image: `${baseUrl}JingfengZhou.jpg`,
    institutions:
      [
        'State Key Laboratory of Cognitive Neuroscience and Learning, Beijing Normal University & Chinese Institute for Brain Research, Beijing'],
    bio:
      [
        "Jingfeng Zhou obtained his Ph.D. from Peking University in 2016. From 2016 to 2021, he conducted postdoctoral research at the NIH/NIDA in the US. Since 2021, he has been an Assistant Investigator Chinese Institute for Brain Research, Beijing. In 2024, he joined the State Key Laboratory of Cognitive Neuroscience and Learning at Beijing Normal University as a professor. His research findings have been published in journals such as Nature, Neuron, Curr Biol, Nat Commun, and J Neurosci. He has received support from various funds, including the NIH K99 Award, the National Natural Science Foundation of China, STI 2030-Major Projects, Chinese Academy of Medical Sciences, and Beijing Nova Program. The group uses rats and mice to study the neural basis of associative learning and decision-making. Specifically, we aim to understand, at the level of systems neuroscience, how the brain constructs associations between events in complex environments and organizes, maintains, updates, and generalizes these learned associations based on experience and current needs, thereby making predictions and guiding choice behaviors. To address these questions, we employ an integrative approach that includes various Pavlovian and operant tasks along with a suite of neuroscience tools, including multichannel in vivo electrophysiology, single- and two-photon imaging, widefield imaging, optogenetics, chemogenetics, neuropharmacology, statistical analysis, machine learning, and computational modeling. Through observing, manipulating, and modeling neuronal activities, we explore the information coding and computational principles of population neurons in regions such as the prefrontal cortex and hippocampus and how they work together to support complex and flexible associative learning, memory, and decision-making behaviors."
      ],
    talkTitle: 'Coordinated hippocampal ensemble dynamics underlie reliable representations of perception and memory'
  },
  {
    name: 'Yu Gu',
    image: `${baseUrl}YuGu.jpg`,
    institutions: [
      'State Key Laboratory of Medical Neurobiology',
      'MOE Frontiers Center for Brain Science',
      'Institutes of Brain Science, Fudan University',
    ],
    bio: [
      "Dr. Yu Gu, graduated from Tsinghua University with B.S. degree in 2007, graduated from University of Maryland with PhD degree in 2014, conducted postdoctoral research in the Department of Neurobiology at Northwestern University from 2014 to 2017, and joined the Institutes of Brain Science at Fudan University as a group leader in 2018. His research results have been published in several leading neuroscience journals, including Nature Neuroscience, Neuron, Cell Reports, eLife, Journal of Neuroscience, etc. He has also received support from several National Natural Science Foundation of China programs and the titles of Shanghai Rising-Star, Young Oriental Scholar and Young Oriental Talent. He is currently a member of the Sensory and Motor Division of the Chinese Society of Neuroscience, a member of the Neurodegenerative Diseases Committee of the Chinese Society for the Promotion of Human Health Science and Technology, and a member of the Young Editorial Board of Neuroscience Bulletin, the journal of the Chinese Society of Neuroscience. His research focuses on experience-dependent remodeling of sensory and cognitive functional circuits. Using in vivo electrophysiology, wide-field calcium imaging, circuit manipulation and other techniques, the group study the regulatory role played by sensory information inputs and the neural mechanisms of cognitive circuital remodeling during the execution of behaviors such as abandonment, predation, and avoidance, and provide guidance for the mechanisms and clinical treatments of related neurological disorders. The group also uses a variety of AI-based neural computational methods to develop novel research techniques for mechanistic studies."
    ],
    talkTitle: 'SimSort: A Powerful Framework for Spike Sorting by Large-Scale Electrophysiology SimulationF'
  },
  {
    name: 'Wei Lin',
    image: `${baseUrl}WeiLin.jpg`,
    institutions:
      [

        'Research Institute of Intelligent Complex Systems (IICS)',
        'Centre for Computational Systems Biology',
        'School of Mathematical Sciences, Fudan University'

      ],
    bio:
      [
        "Wei Lin is a Distinguished Professor and Ph.D. Supervisor at Fudan University, jointly appointed as a Professor at the Gu Chaohao Institute of the Shanghai Center for Mathematical Sciences. His research focuses on biomathematics, complex systems theory, and the mathematical foundations of artificial intelligence, with interdisciplinary applications. He has been awarded the National Science Fund for Distinguished Young Scholars, the Excellent Young Scientists Fund, and has served as the principal investigator of key projects under the National Key Research and Development Program of China. Additionally, he was selected for the Program for New Century Excellent Talents in University of Ministry of Education of China. Professor Lin is a Senior Member of IEEE, Vice Chair of the Biomathematics Committee of the Chinese Mathematical Society, Vice Chair of the Mathematical Life Sciences Committee of CSIAM, and President of the Shanghai Society of Nonlinear Sciences. His contributions have been recognized with numerous honors, including the ICCM Best Paper Award, the V. Afraimovich Award (granted by the International Conference on Nonlinear Science and Complexity to outstanding young scholars in nonlinear physical sciences), the First Prize of the Shanghai Natural Science Award, and the Baosteel Outstanding Teacher Award."
      ],
    talkTitle: 'Rhythmic Evolution and Regulation in Complex Life Systems'
  },
  {
    name: 'Bin Min',
    image: `${baseUrl}BinMin.jpg`,
    institutions:
      [
        'Lin Gang Laboratory'
      ],
    bio:
      [
        "Bin Min is a Young Researcher at Lingang Laboratory, with a specialization in computational cognitive neuroscience. His research primarily focuses on the development of interpretable deep neural network models and innovative data analysis methods aimed at uncovering the neural computational principles underlying high-level cognitive functions, including memory, decision-making, and planning. Furthermore, he is dedicated to advancing and applying cutting-edge technologies, such as brain-inspired computing and brain-machine interface decoding algorithms. His work has been published in prominent journals, including Science, Neuron, and eLife, among others."],
    talkTitle: 'Modeling individual variability of neural computations underlying flexible decisions'
  },
  {
    name: 'Tifei Yuan',
    image: `${baseUrl}TifeiYuan.jpg`,
    institutions: ['School of Psychology, Shanghai Jiao Tong University'],
    bio: [
      "Dr. Tifei Yuan, a Distinguished Professor at Shanghai Jiao Tong University, is the Executive Dean of its School of Psychology and the Shanghai Mental Health Center's Brain Health Institute. He's a recipient of the National Natural Science Foundation of China's Distinguished and Excellent Young Scholar titles. An active figure in academic circles, he's on the editorial boards of multiple scientific journals. Focusing on neural mechanisms and interventions for psychiatric disorders like addiction and depression, he's published in top journals such as Nature Neuroscience, Neuron, Molecular Psychiatry, and Biological Psychiatry."
    ],
    talkTitle: 'Brain stimulation for drug addiction: targeting the prefrontal cortex '
  },
  {
    name: 'Fei Li',
    image: `${baseUrl}FeiLi.jpg`,
    institutions:
      [
        'Department of Developmental and Behavioural Paediatric & Child Primary Care',
        "Ministry of Education - Shanghai Key Laboratory of Children's Environmental Health",
        'Xinhua Hospital, Shanghai Jiao Tong University School of Medicine'
      ],
    bio:
      [
        "Dr. Fei Li completed two rounds of postdoctoral training in Developmental Behavioral Pediatrics and Cognitive Neurobiology at Shanghai Jiao Tong University, Boston University, and Medical College of Georgia. She pursued specialized clinical training in child cognitive and behavioral disorders as a senior visiting scholar at Boston Children's Hospital, Harvard University, and Wisconsin Children's Hospital. Currently, she serves as the Director of the Department of Developmental Behavioral Pediatrics at Xinhua Hospital Affiliated to Shanghai Jiao Tong University School of Medicine, and Director of the Ministry of Education-Shanghai Key Laboratory of Children's Environmental Health. She also holds distinguished professorships at Fudan University's Brain-like Intelligence Research Institute and East China Normal University (School of Psychology and Cognitive Science/Ministry of Education Key Laboratory of Brain Functional Genomics/Co-PI). Her research has been published in prestigious international journals including the American Journal of Psychiatry, Nature Communication, Molecular Psychiatry, and Science Bulletin. She was invited to write a review for the New England Journal of Medicine. Dr. Li has received numerous prestigious grants and awards, including the National Natural Science Foundation's \"Distinguished Young Scholar, \" \"Outstanding Young Scholar, \" Key Project, and China-Canada Government International Cooperation Key Project. She has also been recognized as a Shanghai Leading Talent, Shanghai Outstanding Academic Leader, and Shanghai Shuguang Scholar/Pujiang Scholar. Dr. Li currently serves as the Vice Chair of the Developmental Behavior Group of the Chinese Medical Association's Pediatric Branch, Senior Mentor of the International Society for Autism Research (INSAR, one of 16 globally), and member of the Clinical Innovation and Drug Development Committee of the International College of Neuropsychopharmacology (CINP). She has long been engaged in the tertiary prevention and clinical work of autism spectrum disorder and other neurodevelopmental disorders. Her translational research addresses these challenges through interdisciplinary approaches including neuroimaging, neurotoxicology, epidemiology, and nutrition. She has identified several high-risk factors for autism and its common comorbidities, clarified exposure injury mechanisms, and established early prediction and intervention biomarkers. Dr. Li actively develops and implements new intervention strategies that integrate medical treatment and prevention."
      ],
    talkTitle: 'Autism Etiology, Early Diagnosis, and Comprehensive Intervention: Translational Research from Bench to Bedside'
  },
  {
    name: `Tingyong Feng`,
    image: `${baseUrl}TingyongFeng.jpg`,
    institutions:
      [
        'Faculty of Psychology, Southwest University',
        'Key Laboratory of Cognition and Personality, Ministry of Education'
      ],
    bio:
      [
        "Tingyong Feng, Ph.D., second-level professor of Southwest University, Young Changjiang Scholar of the Ministry of Education, Vice Dean of Faculty of Psychology, director of the National Experimental Teaching Demonstration Center of Psychology, expert of the National Student Mental Health Advisory Committee of the Ministry of Education (Deputy leader of the Southwest region), deputy editor of journal of Psychological Science. Editorial board member of core journals such as Advances in Psychological Science and Applied Psychology; Vice Chairman of the Professional Committee of Emotional and Health Psychology, Chinese Psychological Society, and vice Chairman of the Cognitive Neurobiology Branch of Chinese Neuroscience Society. His research interests focus on cognitive neural mechanisms of temporal cognition and decision making (focusing on procrastination behavior and intertemporal choice) and cognitive development and brain plasticity in children (both normal and ADHD/ASD children). At present, he has presided over 5 National Natural Science Foundation projects of China, 1 major special project of the Ministry of Science and Technology, 1 key research and development project of the Ministry of Science and Technology, and 1 key project of Chongqing Technology innovation application and development. As the first or corresponding author in the top international journals of Psychology, such as Nature Communications, Journal of Experimental Psychology: General and Neuroimage, have published more than 150 research results in high-level professional journals (more than 90 SCI/SSCI papers), edited or deputy edited 6 national planning textbooks and books, and their research results have been selected as Editor's Choice in Science."
      ],
    talkTitle: 'Neural basis and neuromodulation on procrastination'
  },
  {
    name: `Huiguang He`,
    image: `${baseUrl}HuiguangHe.jpg`,
    institutions:
      [
        'State Key Laboratory of Brain Cognition and Brain-inspired Intelligence Technology',
        'Institute of Automation, Chinese Academy of Sciences'
      ],
    bio:
      [
        "Huiguang He received the B.S. degree (1994) and the M.S. degree (1997) from Dalian Maritime University (DMU), Dalian, China. He got the Ph.D. degree (with honor) in pattern recognition and intelligent systems from Institute of Automation, Chinese Academy of Sciences (CASIA), Beijing, China. He is currently a full professor with CASIA. He was an Associate Lecturer in DMU from 1997 to 1999, and postdoctoral researcher in University of Rochester, USA from 2003 to 2004. He was a visiting professor in University of North Carolina at Chapel Hill from 2014 to 2015. He won the following awards: Excellent Ph.D. dissertation of CAS (2004), National Science & Technology Award (2003, 2004), Beijing Science & Technology Award (2002, 2003), K.C. Wong Education Prizes (2007, 2009), “Jia-Xi Lu” Young Talent Prize (2009) and excellent member of Youth Innovation Promotion Association, CAS (2016). His research interests include pattern recognition, medical image processing, and brain computer interface (BCI). His research has been supported by several research grants from National Science Foundation of China, and he has published more than 200 peer-reviewed papers. "
      ],
    talkTitle: 'Fine-grained Emotion Neural Encoding and Decoding'
  },
  {
    name: `Haiyan Wu`,
    image: `${baseUrl}HaiyanWu.jpg`,
    institutions:
      [
        'Affective, Neuroscience, and Decision-making Lab (ANDlab)',
        'Center for Cognitive and Brain Sciences',
        'Institute of Collaborative Innovation, University of Macau'
      ],
    bio:
      [

        "Professor Haiyan Wu is the Principal Investigator of ANDlab at the University of Macau (https://andlab-um.com/) and a Visiting Associate at the California Institute of Technology. Before joining the University of Macau, Professor Wu obtained her Ph.D. from Beijing Normal University and served as a researcher at the Institute of Psychology, Chinese Academy of Sciences. Her research focuses on developing an interdisciplinary technological framework that integrates artificial intelligence, brain imaging, computational models, intracranial and extracranial neural signals, neural modulation, virtual reality, and big data to explore social neuroscience. Her work primarily centers on the interaction between emotion and decision-making in the brain. has more than 50 publications in neuroscience and psychology journals such as eLife, Neuroscience & Biobehavioral Reviews, Scientific Data, Neuroimage, Annals of the New York Academy of Sciences, Human Brain Mapping, and her publications have been cited over 2100 times. She is a member of the Young Editorial Board of Neuroscience Bulletin. In 2023, she was recognized as one of the \"Top 30 Young Innovators in Brain Science and Artificial Intelligence.\""

      ],
    talkTitle: 'Unethical amnesia brain: Memory and metacognitive distortion induced by dishonesty'
  },
]

const selectedSpeaker = ref(speakers[0])
const route = useRoute();
onMounted(() => {
  if (route.query.speaker) {
    const speakerName = route.query.speaker.toString();
    // 使用不区分大小写的比较，忽略前后空格
    const foundSpeaker = speakers.find(s =>
      s.name.trim().toLowerCase() === speakerName.trim().toLowerCase());

    if (foundSpeaker) {
      selectedSpeaker.value = foundSpeaker;
    }
  }
});

// 监听路由变化
watch(
  () => route.query.speaker,
  (newSpeakerName) => {
    if (newSpeakerName) {
      const speakerNameStr = newSpeakerName.toString();
      const foundSpeaker = speakers.find(s =>
        s.name.trim().toLowerCase() === speakerNameStr.trim().toLowerCase());

      if (foundSpeaker) {
        selectedSpeaker.value = foundSpeaker;
        // 可选：滚动到详情区域
        document.querySelector('.speaker-details')?.scrollIntoView({ behavior: 'smooth' });
      }
    }
  }
);
</script>

<style scoped>
.speaker-container {
  display: flex;
  min-height: calc(100vh - 80px);
  padding: 20px 0;
  max-width: 1400px;
  margin: 0 auto;
}

.sidebar {
  width: 250px;
  background-color: #f5f5f5;
  border-radius: 8px;
  padding: 20px;
  margin-right: 30px;
  height: fit-content;
  position: sticky;
  top: 100px;
}

.sidebar-title {
  font-size: 1.5rem;
  margin-bottom: 20px;
  padding-bottom: 10px;
  border-bottom: 1px solid #e0e0e0;
  color: #333;
}

.speaker-list {
  list-style: none;
  padding: 0;
}

.speaker-list li {
  padding: 12px 15px;
  margin-bottom: 5px;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.speaker-list li:hover {
  background-color: #e8e8e8;
}

.speaker-list li.active {
  background-color: #2c3e50;
  color: white;
}

.speaker-details {
  flex: 1;
  padding: 30px;
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.1);
}

.details-content {
  animation: fadeIn 0.5s ease;
}

.speaker-name {
  font-size: 2rem;
  margin-bottom: 5px;
  color: #2c3e50;
}


.talk-title {
  margin: 20px 0;
  padding: 15px;
  background-color: #f8f9fa;
  border-left: 4px solid #2c3e50;
  border-radius: 4px;
}

.talk-title h3 {
  margin: 0 0 8px 0;
  font-size: 1.4rem;
  color: #2c3e50;
}

.speaker-info {
  display: flex;
  margin-bottom: 30px;
}

.speaker-image-container {
  width: 200px;
  margin-right: 30px;
}

.speaker-image {
  width: 100%;
  height: auto;
  border-radius: 8px;
  box-shadow: 0 3px 10px rgba(0, 0, 0, 0.1);
}

.speaker-bio {
  flex: 1;
}

.speaker-bio p {
  margin-bottom: 15px;
  line-height: 1.6;
  color: #444;
}

.speaker-contact {
  padding-top: 20px;
  border-top: 1px solid #e0e0e0;
}

.speaker-contact p {
  margin-bottom: 10px;
  color: #555;
}

.select-prompt {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 300px;
  font-size: 1.2rem;
  color: #888;
  text-align: center;
}

.institutions {
  margin-top: 10px;
}

.institution-list {
  margin: 5px 0 0 0;
  padding-left: 20px;
}

.institution-list li {
  margin-bottom: 5px;
  color: #555;
}

/* 单个机构的情况，可以直接显示而不用列表 */
.institution-list:only-child {
  display: inline;
  margin-left: 5px;
  padding-left: 0;
  list-style: none;
}

.institution-list:only-child li {
  display: inline;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }

  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* 响应式设计 */
@media (max-width: 768px) {
  .speaker-container {
    flex-direction: column;
  }

  .sidebar {
    width: 100%;
    margin-right: 0;
    margin-bottom: 20px;
    position: static;
  }

  .speaker-list {
    display: flex;
    overflow-x: auto;
    padding-bottom: 10px;
  }

  .speaker-list li {
    white-space: nowrap;
    margin-right: 10px;
    margin-bottom: 0;
  }

  .speaker-info {
    flex-direction: column;
  }

  .speaker-image-container {
    width: 100%;
    max-width: 250px;
    margin: 0 auto 20px;
  }

  .speaker-details {
    padding: 20px;
  }
}
</style>
