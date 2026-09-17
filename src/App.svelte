<script>
  import resume from "./assets/resume_jeremic.pdf";
  import { onMount } from "svelte";
  import Hydra from "hydra-synth";
  import moduleCollage from "./assets/modulecollage.png";
  import xenakisPaper from "./assets/xenakisPaper.pdf";
  import galleryImage1 from "./assets/img1.jpeg";
  import galleryImage2 from "./assets/img2.jpeg";
  import galleryImage3 from "./assets/img3.jpeg";
  import galleryImage4 from "./assets/img4.jpeg";

  /** @type {HTMLCanvasElement | null} */
  let hydraCanvas = null;

  onMount(() => {
    if (!hydraCanvas) return;

    const pixelRatio = Math.min(window.devicePixelRatio || 1, 2);

    const hydra = /** @type {any} */ (
      new Hydra(
        /** @type {any} */ ({
          canvas: hydraCanvas,
          detectAudio: false,
          makeGlobal: false,
        }),
      ).synth
    );

    const resizeHydra = () => {
      hydra.setResolution(
        window.innerWidth * pixelRatio,
        window.innerHeight * pixelRatio,
      );
    };

    resizeHydra();
    window.addEventListener("resize", resizeHydra);

    hydra
      .solid(0, 0, 0)
      .layer(
        hydra
          .noise(1)
          .kaleid(5)
          .modulatePixelate(
            hydra.noise(() => Math.sin(hydra.time * 0.1)).pixelate(1024),
            512,
          )
          .thresh(0.5)
          .luma(0.5)
          .colorama(3),
      )
      .modulateScale(
        hydra
          .noise(1)
          .kaleid(5)
          .thresh(() => Math.sin(hydra.time))
          .luma(0.5)
          .colorama(1),
      )
      .out();

    return () => {
      window.removeEventListener("resize", resizeHydra);
    };
  });

  const tabs = document.getElementsByClassName("sectionHeader");
  const audio = {
    ambient: {
      index: 0,
      songs: [
        "https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/soundcloud%253Atracks%253A2140730886&color=%230c0c0c&auto_play=false&hide_related=false&show_comments=true&show_user=true&show_reposts=false&show_teaser=true",
        "https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/soundcloud%253Atracks%253A2140723386&color=%230c0c0c&auto_play=false&hide_related=false&show_comments=true&show_user=true&show_reposts=false&show_teaser=true",
        "https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/soundcloud%253Atracks%253A2140713096&color=%230c0c0c&auto_play=false&hide_related=false&show_comments=true&show_user=true&show_reposts=false&show_teaser=true",
      ],
    },
  };
  const audioGenres = Object.keys(audio);
  const audioProjects = [
    {
      title: "My music",
      description:
        "I've been producing music since 2022, and have explored various different genres. My main DAW is Ableton though I have worked with Logic and FL Studio prior to switching. Most of the music I produce can be broadly labelled as",
      genres: ["Ambient", "Footwork", "Breakcore"],
      footer:
        "Click on the genre to hear an example! You'll get a new one everytime you click.",
      discographyUrl: "https://soundcloud.com/sport-audio1",
    },
    {
      title: "DJing",
      description:
        "I like to DJ and to help organise events. We hosted a freeparty in Cyprus with over 200 atendees, bring Hardcore to Cyprus.",
    },
    {
      title: "Live performance",
      description:
        "I am a classically trained saxophonist and have been playing since I was 10 years old. Through this I've had the priveling of playing for the Limassol Wind Orchestra while I was in Cyprus. At Berklee, I focused more on jazz, and got the opportunity to play at the Berklee Performance Cetner with the Tia Fuller big band, where we performed Duke Ellington's arrangement of the Nutcracker suite.\n Apart from saxophone, I also like to do live performances with Ableton and other musicians, often doing setups with a guitar and my laptop for long, ambient, live looping sets.",
      link: "https://youtu.be/lxmOSHKgMT4",
      linkLabel: "An example of an modular live looping piece i did",
    },
  ];
  const audioGalleryImages = [
    galleryImage1,
    galleryImage2,
    galleryImage3,
    galleryImage4,
  ];
  const videos = {
    video1: {
      url: "https://youtube.com/embed/kVsvYNYZnN0",
      description:
        "This installation was an exploration of my relationship with language and expression of thought. Growing up in a multilingual environment forced people to find different ways to express their thoughts when words weren’t an option. I wanted to explore this through the lens of graffiti, a form of expression that is taboo yet so visible. The video features a variety of media covering the topic of graffiti and its censorship, and various mouths saying sentences in various languages transliterated to english.",
    },
    video2: {
      url: "https://youtube.com/embed/nrp1Hta4BV0",
      description:
        "I've worked with various artists in the area, using touchdesigner for their live shows, below is a reel of various concerts settings and visuals that I've designed.",
    },
    video3: {
      url: "https://youtube.com/embed/pd5wMqWBaBw",
      description:
        "This is an edit I made using various processing technqiues in touchdesigner, most of the processing consists of feedback lines, using delay lines in similar ways to how a reverb works. All the clips were recorded during a roadtrip through the West coast in May/June of 2026. ",
    },
  };

  const softwareProjects = [
    {
      title: "e-baton",
      description:
        "The e-baton is a NIME (New Instrument for Music Expression) that I built in late 2025 to bridge the gap between the gestural communication of conductors and electronic music. Using an accelerometer for expansive feature expression, I created a conducting-like interface that can communicate with Max. This project was demoed at NIME 2026 and published in the NIME 2026 proceedings.",
      link: "https://nime.org/proc/nime2026_163/index.html",
      linkLabel: "Read the paper",
      video: "https://youtube.com/embed/gltICGnNnk4",
    },
    {
      title: "Earth Mission Control - Coral Quest",
      description:
        "Earth Mission Control - Coral Quest was the final project of a Harvard-MIT-Berklee collaboration class. Our group of four created a custom vignette for the MIT Media Lab Future Worlds project Earth Mission Control, a VR experience where users explore a coral reef and learn how climate change and pollution damage the ocean. I designed most of the visual aspects and wrote scripts for the interactions.",
      video: "https://youtube.com/embed/tZg8_fqa_eQ",
    },
    {
      title: "Building Eurorack Modules",
      description:
        "For my capstone project at Berklee, I designed and prototyped two Eurorack modules. The first module was a simple 8HP reverb module, designed with the intention of providing a wide variety of reverb sounds in a small size through a macro knob that controlled multiple parameters of a custom FDN reverb algorithm I designed. The second module was a fully analog Turing machine inspired random voltage generator which provided a variety of ways to trigger and manipulate the output. Both modules had custom PCBS designed in KiCAD and were assembled with custom designed front panels.",
      image: moduleCollage,
    },
    {
      title: "Projects in Max",
      description:
        "I've worked extensively in Max in a variety of applications. Some of my favorite projects include",
      items: [
        {
          text: "A recreation of the stochastic composition algorithm used by Iannis Xenakis in his composition 'Achorripsis', which I used to generate audio and video parts for a short film I created",
          link: xenakisPaper,
          linkLabel: "Read the paper",
        },
        {
          text: "A custom performance patch and interface for my instrument, the e-baton, which I used to perform at Berklee and demo at NIME 2026",
        },
        {
          text: "A generative patch that used MIDI file input to create Markov chains to output real-time generative counterpoint based on the input, inspired by Lejaren Hiller's Illiac Suite",
        },
        {
          text: "A generative performance patch controlled using hand gestures with Mediapipe, feauturing visual feedback and processing. This was my midterms project for Introduction to Max",
          link: "https://youtu.be/W2sRp3mzC5A",
          linkLabel: "Watch the video",
        },
        {
          text: "A custom FDN reverb algortihm that I designed in gen~, which I used in my Eurorack reverb through the Daisy Path SM board",
          link: "https://youtu.be/S02sN2khSyI",
          linkLabel: "Examples of the reverb",
        },
      ],
    },
    {
      title: "Web Projects",
      description: "A selection of web and browser-based projects.",
      items: [
        {
          text: "I built this website myself. It is built with Svelte and uses Hydra for the background visuals.",
        },
        {
          text: "I also built the backend for Soundscape Studios, a studio I interned at in Cyprus. The website runs on Wix, but uses the Velo API for a custom booking system with automated emails, data collection, and approval of incoming mix demos.",
        },
        {
          text: "Fusionem Artis, an installation I built in 2025, runs as an Electron Web App with Hydra, Csound, and MediaPipe.",
          link: "https://youtu.be/NsNPWxWmPzs",
          linkLabel: "A short demo of the installation",
        },
        {
          text: "What's this Code? is a browser extension I built for HackDartmouth 2025 that translates code between languages using the Gemini API.",
          link: "https://github.com/BBavoso/whats-this-code",
          linkLabel: "Project repository",
        },
      ],
    },
  ];
  let activeVideo = "video1";
  let activeVideoIndex = 1;
  let activeAudioIndex = 0;
  let activeGalleryIndex = 0;
  let activeSoftwareIndex = 0;

  function changeVideo(direction) {
    const videoHolder = document.getElementById("video2");
    activeVideoIndex = ((activeVideoIndex - 1 + direction) % 3) + 1;
    activeVideo = `video${activeVideoIndex}`;
    const videoDisplay = /** @type {HTMLIFrameElement | null} */ (
      document.querySelector("#videoPlayer")
    );
    const videoDesc = document.getElementById("videoDescription");
    if (!videoDisplay || !videoDesc) return;
    videoDisplay.src = videos[activeVideo].url;
    videoDesc.textContent = videos[activeVideo].description;
  }

  function changeSoftware(direction) {
    activeSoftwareIndex =
      (activeSoftwareIndex + direction + softwareProjects.length) %
      softwareProjects.length;
  }

  /** @param {string} genre */
  function loadAudio(genre) {
    const selectedAudio = /** @type {any} */ (audio)[genre];
    const audioPlayer = /** @type {HTMLIFrameElement | null} */ (
      document.getElementById("audioPlayer")
    );

    if (!selectedAudio || !audioPlayer) return;

    selectedAudio.index =
      (selectedAudio.index + 1) % selectedAudio.songs.length;
    audioPlayer.src = selectedAudio.songs[selectedAudio.index];
  }

  /** @param {number} direction */
  function changeAudio(direction) {
    activeAudioIndex =
      (activeAudioIndex + direction + audioProjects.length) %
      audioProjects.length;
    if (activeAudioIndex === 1) {
      activeGalleryIndex = 0;
      loadSection("audioGallery");
    } else {
      loadMenu("audioGallery");
    }
  }

  /** @param {number} direction */
  function changeGalleryImage(direction) {
    activeGalleryIndex =
      (activeGalleryIndex + direction + audioGalleryImages.length) %
      audioGalleryImages.length;
  }
  function draggable(node) {
    let x = 0,
      y = 0;

    function clampPosition(
      parent,
      left,
      top,
      width = parent.offsetWidth,
      height = parent.offsetHeight,
    ) {
      const maxLeft = Math.max(0, window.innerWidth - width);
      const maxTop = Math.max(0, window.innerHeight - height);

      return {
        left: Math.min(Math.max(left, 0), maxLeft),
        top: Math.min(Math.max(top, 0), maxTop),
      };
    }

    function handleMouseDown(e) {
      const rect = node.parentElement.getBoundingClientRect();
      document.body.style.userSelect = "none";
      x = e.clientX - rect.left;
      y = e.clientY - rect.top;

      function handleMouseMove(moveEvent) {
        const parent = node.parentElement;
        const nextPosition = clampPosition(
          parent,
          moveEvent.clientX - x,
          moveEvent.clientY - y,
        );

        parent.style.left = nextPosition.left + "px";
        parent.style.top = nextPosition.top + "px";
      }

      function handleMouseUp() {
        document.body.style.userSelect = "auto";
        window.removeEventListener("mousemove", handleMouseMove);
        window.removeEventListener("mouseup", handleMouseUp);
      }

      window.addEventListener("mousemove", handleMouseMove);
      window.addEventListener("mouseup", handleMouseUp);
    }

    function handleResizeStart(e) {
      e.stopPropagation();
      const parent = node.parentElement;
      const startX = e.clientX;
      const startY = e.clientY;
      const startWidth = parent.offsetWidth;
      const startHeight = parent.offsetHeight;
      const startLeft = parent.offsetLeft;
      const startTop = parent.offsetTop;

      function handleMouseMove(moveEvent) {
        document.body.style.userSelect = "none";

        const deltaX = moveEvent.clientX - startX;
        const deltaY = moveEvent.clientY - startY;

        const width = Math.max(200, startWidth - deltaX);
        const height = Math.max(200, startHeight - deltaY);
        const nextPosition = clampPosition(
          parent,
          startLeft + deltaX,
          startTop + deltaY,
          width,
          height,
        );

        parent.style.width = width + "px";
        parent.style.height = height + "px";
        parent.style.left = nextPosition.left + "px";
        parent.style.top = nextPosition.top + "px";
      }

      function handleMouseUp() {
        document.body.style.userSelect = "auto";
        window.removeEventListener("mousemove", handleMouseMove);
        window.removeEventListener("mouseup", handleMouseUp);
      }

      window.addEventListener("mousemove", handleMouseMove);
      window.addEventListener("mouseup", handleMouseUp);
    }

    node.addEventListener("mousedown", handleMouseDown);
    const resizeHandle = node.parentElement?.querySelector(".resize-handle");
    if (resizeHandle) {
      resizeHandle.addEventListener("mousedown", handleResizeStart);
    }

    return {
      destroy() {
        node.removeEventListener("mousedown", handleMouseDown);
        if (resizeHandle) {
          resizeHandle.removeEventListener("mousedown", handleResizeStart);
        }
      },
    };
  }

  let showBackgroundVideo = true;

  const sectionLayouts = {
    about: {
      fullscreen: {
        top: "0",
        left: "0",
        width: "100vw",
        height: "100vh",
        overflow: "auto",
      },
      open: {
        top: "5vh",
        left: "44vw",
        width: "56vw",
        height: "60vh",
        overflow: "auto",
      },
      closed: {
        top: "0",
        left: "0",
        width: "0",
        height: "0",
        overflow: "hidden",
      },
    },
    video1: {
      fullscreen: {
        top: "0",
        left: "0",
        width: "100vw",
        height: "100vh",
        overflow: "auto",
      },
      open: {
        top: "40vh",
        left: "10vw",
        width: "26vw",
        height: "60vh",
        overflow: "auto",
      },
      closed: {
        top: "0",
        left: "0",
        width: "0",
        height: "0",
        overflow: "hidden",
      },
    },
    video2: {
      fullscreen: {
        top: "0",
        left: "0",
        width: "100vw",
        height: "100vh",
        overflow: "auto",
      },
      open: {
        top: "15vh",
        left: "54vw",
        width: "46vw",
        height: "50vh",
        overflow: "auto",
      },
      closed: {
        top: "0",
        left: "0",
        width: "0",
        height: "0",
        overflow: "hidden",
      },
    },
    software2: {
      fullscreen: {
        top: "0",
        left: "0",
        width: "100vw",
        height: "100vh",
        overflow: "auto",
      },
      open: {
        top: "0vh",
        left: "25vw",
        width: "25vw",
        height: "40vh",
        overflow: "auto",
      },
      closed: {
        top: "0",
        left: "0",
        width: "0",
        height: "0",
        overflow: "hidden",
      },
    },
    software1: {
      fullscreen: {
        top: "0",
        left: "0",
        width: "100vw",
        height: "100vh",
        overflow: "auto",
      },
      open: {
        top: "0vh",
        left: "50vw",
        width: "50vw",
        height: "100vh",
        overflow: "auto",
      },
      closed: {
        top: "0",
        left: "0",
        width: "0",
        height: "0",
        overflow: "hidden",
      },
    },
    audio1: {
      fullscreen: {
        top: "0",
        left: "0",
        width: "100vw",
        height: "100vh",
        overflow: "auto",
      },
      open: {
        top: "5vh",
        left: "44vw",
        width: "26vw",
        height: "60vh",
        overflow: "auto",
      },
      closed: {
        top: "0",
        left: "0",
        width: "0",
        height: "0",
        overflow: "hidden",
      },
    },
    audio2: {
      fullscreen: {
        top: "0",
        left: "0",
        width: "100vw",
        height: "100vh",
        overflow: "auto",
      },
      open: {
        top: "80vh",
        width: "100vw",
        height: "20vh",
        overflow: "auto",
      },
      closed: {
        top: "0",
        left: "0",
        width: "0",
        height: "0",
        overflow: "hidden",
      },
    },
    audioGallery: {
      fullscreen: {
        top: "0",
        left: "0",
        width: "100vw",
        height: "100vh",
        overflow: "auto",
      },
      open: {
        top: "0vh",
        left: "0vw",
        width: "40vw",
        height: "50vh",
        overflow: "hidden",
      },
      closed: {
        top: "0",
        left: "0",
        width: "0",
        height: "0",
        overflow: "hidden",
      },
    },
    cv: {
      fullscreen: {
        top: "0",
        left: "0",
        width: "100vw",
        height: "100vh",
        overflow: "auto",
      },
      open: {
        top: "5vh",
        left: "10vw",
        width: "80vw",
        height: "85vh",
        overflow: "auto",
      },
      closed: {
        top: "0",
        left: "0",
        width: "0",
        height: "0",
        overflow: "hidden",
      },
    },
  };

  const defaultSectionLayout = {
    open: {
      top: "0",
      left: "0",
      width: "100vw",
      height: "100vh",
      overflow: "auto",
    },
    closed: {
      top: "0",
      left: "0",
      width: "0",
      height: "0",
      overflow: "hidden",
    },
  };

  const sectionTransition = "height 0.5s ease-in-out, width 0.5s ease-in-out";
  const sectionTransitionDuration = 500;

  function setBackgroundToggleVisible(isVisible) {
    const toggle = /** @type {HTMLLabelElement | null} */ (
      document.querySelector(".bg-toggle")
    );

    if (!toggle) {
      return;
    }

    toggle.classList.toggle("hidden", !isVisible);
  }

  function applySectionLayout(targetSection, layout) {
    targetSection.style.transition = sectionTransition;
    targetSection.style.opacity = "1";
    targetSection.style.top = layout.top;
    targetSection.style.left = layout.left;
    targetSection.style.width = layout.width;
    targetSection.style.height = layout.height;
    targetSection.style.overflow = layout.overflow;

    setTimeout(() => {
      targetSection.style.transition = "none";
    }, sectionTransitionDuration);
  }

  function loadSection(sectionId) {
    if (sectionId === "video") {
      loadSection("video1");
      loadSection("video2");
      return;
    }
    if (sectionId === "audio") {
      loadSection("audio1");
      loadSection("audio2");
      if (activeAudioIndex === 1) {
        loadSection("audioGallery");
      }
      return;
    }
    if (sectionId === "software") {
      loadSection("software1");
      loadSection("software2");
      return;
    }
    if (sectionId === "cv") {
      const targetSection = /** @type {HTMLElement | null} */ (
        document.getElementById(sectionId)
      );

      if (!targetSection) {
        return;
      }

      const closeButton = /** @type {HTMLButtonElement | null} */ (
        targetSection.querySelector(".close-button")
      );
      const layout = sectionLayouts[sectionId] ?? defaultSectionLayout;

      targetSection.classList.add("open");
      targetSection.classList.add("fullscreen");
      applySectionLayout(targetSection, layout.fullscreen);
      if (closeButton) {
        closeButton.style.opacity = "1";
      }
      return;
    }
    const targetSection = /** @type {HTMLElement | null} */ (
      document.getElementById(sectionId)
    );

    if (!targetSection) {
      return;
    }

    const links = [...document.getElementsByClassName("loaded")];
    const closeButton = /** @type {HTMLButtonElement | null} */ (
      targetSection.querySelector(".close-button")
    );
    const layout = sectionLayouts[sectionId] ?? defaultSectionLayout;
    const keepMenuVisible = true;

    if (!keepMenuVisible) {
      links.forEach((link) => {
        link.classList.remove("loaded");
        link.classList.add("unloaded");
      });

      setBackgroundToggleVisible(false);
    }

    if (sectionId === "about") {
      const sectionText = /** @type {HTMLElement | null} */ (
        targetSection.querySelector(".sectionText")
      );

      if (sectionText) {
        sectionText.classList.remove("about-text-enter");
        void sectionText.offsetWidth;
        sectionText.classList.add("about-text-enter");
      }
    }

    targetSection.classList.add("open");
    applySectionLayout(targetSection, layout.open);
    if (closeButton) {
      closeButton.style.opacity = "1";
    }
  }

  function toggleSectionFullscreen(sectionId) {
    const targetSection = document.getElementById(sectionId);
    if (!targetSection) return;

    const layout = sectionLayouts[sectionId];
    if (!layout) return;

    const closeButton = /** @type {HTMLButtonElement | null} */ (
      targetSection.querySelector(".close-button")
    );
    const isFullscreen = targetSection.classList.toggle("fullscreen");

    const flag = isFullscreen ? layout.fullscreen : layout.open;

    // Set transition for both entering and exiting fullscreen
    targetSection.style.transition = sectionTransition;

    if (flag == layout.fullscreen) {
      applySectionLayout(targetSection, layout.fullscreen);
    } else {
      applySectionLayout(targetSection, layout.open);
    }

    setTimeout(() => {
      targetSection.style.transition = "none";
    }, sectionTransitionDuration);

    if (closeButton) {
      closeButton.style.opacity = "1";
    }
  }

  function loadMenu(sectionId) {
    const targetSection = /** @type {HTMLElement | null} */ (
      document.getElementById(sectionId)
    );

    if (!targetSection) {
      return;
    }

    const closeButton = /** @type {HTMLButtonElement | null} */ (
      targetSection.querySelector(".close-button") //
    );
    const layout = sectionLayouts[sectionId] ?? defaultSectionLayout;

    targetSection.style.transition = "none";
    targetSection.style.top = layout.closed.top;
    targetSection.style.left = layout.closed.left;
    targetSection.style.width = layout.closed.width;
    targetSection.style.height = layout.closed.height;
    targetSection.style.opacity = "0";
    targetSection.style.overflow = layout.closed.overflow;
    targetSection.classList.remove("open");
    targetSection.classList.remove("fullscreen");

    if (sectionId === "about") {
      targetSection
        .querySelector(".sectionText")
        ?.classList.remove("about-text-enter");
    }

    if (closeButton) {
      closeButton.style.opacity = "0";
    }

    setBackgroundToggleVisible(true);
  }
</script>

<!-- <label class="bg-toggle" for="bg-video-toggle">
  <span>Background</span>
  <input
    class="bg-toggle-input"
    id="bg-video-toggle"
    type="checkbox"
    bind:checked={showBackgroundVideo}
    aria-label="Toggle background video"
  />
  <span class="bg-toggle-slider" aria-hidden="true"></span>
</label> -->

<div id="sections">
  <h1 class="loaded">Marko Jeremic</h1>
  <button type="button" on:click={() => loadSection("about")}
    ><h2 class="loaded">About Me</h2></button
  >
  <button type="button" on:click={() => loadSection("software")}
    ><h2 class="loaded">Software/Hardware</h2></button
  >
  <button type="button" on:click={() => loadSection("video")}
    ><h2 class="loaded">Video</h2></button
  >
  <button type="button" on:click={() => loadSection("audio")}
    ><h2 class="loaded">Audio</h2></button
  >
  <button type="button" on:click={() => loadSection("cv")}
    ><h2 class="loaded">CV</h2></button
  >
</div>

<div id="video1" class="details">
  <div class="sectionHeader" use:draggable>
    <h3 class="resize-handle">&#10529</h3>
    <span class="sectionHeaderTitle">videodesc.txt</span>

    <div class="sectionHeaderControls">
      <button
        class="fullscreen-button"
        type="button"
        on:click={() => toggleSectionFullscreen("video1")}
        ><h3>&#9974</h3>
      </button>
      <button
        class="close-button"
        type="button"
        on:click={() => loadMenu("video1")}
        ><h3>&#88</h3>
      </button>
    </div>
  </div>
  <div id="videoDescription">
    This installation was an exploration of my relationship with language and
    expression of thought. Growing up in a multilingual environment forced
    people to find different ways to express their thoughts when words weren’t
    an option. I wanted to explore this through the lens of graffiti, a form of
    expression that is taboo yet so visible. The video features a variety of
    media covering the topic of graffiti and its censorship, and various mouths
    saying sentences in various languages transliterated to english.
  </div>
  <button class="videoNav" id="backward" on:click={() => changeVideo(-1)}>
    &lt;&lt;
  </button>
  <button class="videoNav" id="forward" on:click={() => changeVideo(1)}>
    &gt;&gt;
  </button>
</div>

<div id="video2" class="details">
  <div class="sectionHeader" use:draggable>
    <h3 class="resize-handle">&#10529</h3>
    <span class="sectionHeaderTitle">video.mp4</span>

    <div class="sectionHeaderControls">
      <button
        class="fullscreen-button"
        type="button"
        on:click={() => toggleSectionFullscreen("video2")}
        ><h3>&#9974</h3>
      </button>
      <button
        class="close-button"
        type="button"
        on:click={() => loadMenu("video2")}
        ><h3>&#88</h3>
      </button>
    </div>
  </div>
  <iframe
    title="Video Player"
    id="videoPlayer"
    src={videos[activeVideo].url}
    frameborder="0"
    allowfullscreen
  ></iframe>
</div>

<div id="audio1" class="details">
  <div class="sectionHeader" use:draggable>
    <h3 class="resize-handle">&#10529</h3>
    <span class="sectionHeaderTitle">music.mj3</span>

    <div class="sectionHeaderControls">
      <button
        class="fullscreen-button"
        type="button"
        on:click={() => toggleSectionFullscreen("audio1")}
        ><h3>&#9974</h3>
      </button>
      <button
        class="close-button"
        type="button"
        on:click={() => loadMenu("audio1")}
        ><h3>&#88</h3>
      </button>
    </div>
  </div>
  <div class="sectionText">
    <h3>{audioProjects[activeAudioIndex].title}</h3>
    <div>
      {audioProjects[activeAudioIndex].description}
      {#if audioProjects[activeAudioIndex].genres}
        {#each audioGenres as genre, index}
          <button
            class="genreButton"
            type="button"
            on:click={() => loadAudio(genre)}>{genre}</button
          >{index < audioGenres.length - 1 ? ", " : "."}
        {/each}
      {/if}
    </div>
    {#if audioProjects[activeAudioIndex].footer}
      <div>{audioProjects[activeAudioIndex].footer}</div>
    {/if}
    {#if audioProjects[activeAudioIndex].discographyUrl}
      <div>
        My entire discography can be found
        <a href={audioProjects[activeAudioIndex].discographyUrl} target="_blank"
          >here</a
        >
      </div>
    {/if}
    {#if audioProjects[activeAudioIndex].link}
      <div>
        <a
          href={audioProjects[activeAudioIndex].link}
          target="_blank"
          rel="noreferrer"
        >
          {audioProjects[activeAudioIndex].linkLabel || "Read more"}
        </a>
      </div>
    {/if}
  </div>
  <button class="audioNav" id="audioBackward" on:click={() => changeAudio(-1)}>
    &lt;&lt;
  </button>
  <button class="audioNav" id="audioForward" on:click={() => changeAudio(1)}>
    &gt;&gt;
  </button>
  <div class="resize-handle"></div>
</div>
<div id="audio2" class="details">
  <div class="sectionHeader" use:draggable style="margin-bottom: 0px">
    <h3 class="resize-handle">&#10529</h3>
    <span class="sectionHeaderTitle">audioplayer.html</span>

    <div class="sectionHeaderControls">
      <button
        class="fullscreen-button"
        type="button"
        on:click={() => toggleSectionFullscreen("audio2")}
        ><h3>&#9974</h3>
      </button>
      <button
        class="close-button"
        type="button"
        on:click={() => loadMenu("audio2")}
        ><h3>&#88</h3>
      </button>
    </div>
  </div>
  <div class="sectionText" id="audioPlayerContainer">
    <iframe
      id="audioPlayer"
      title="SoundCloud Player"
      width="100%"
      height="166"
      scrolling="no"
      frameborder="no"
      allow="autoplay; encrypted-media"
      src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/soundcloud%253Atracks%253A2140730886&color=%230c0c0c&auto_play=false&hide_related=false&show_comments=true&show_user=true&show_reposts=false&show_teaser=true"
    ></iframe>
    <div
      style="font-size: 10px; color: #cccccc;line-break: anywhere;word-break: normal;overflow: hidden;white-space: nowrap;text-overflow: ellipsis; font-family: Interstate,Lucida Grande,Lucida Sans Unicode,Lucida Sans,Garuda,Verdana,Tahoma,sans-serif;font-weight: 100;"
    >
      <a
        href="https://soundcloud.com/sport-audio1"
        title="sport fm online library"
        target="_blank"
        style="color: #cccccc; text-decoration: none;"
        >sport fm online library</a
      >
    </div>
  </div>
</div>
<div style="position: absolute; top: 10px left:500px"></div>
<div id="audioGallery" class="details">
  <div class="sectionHeader" use:draggable>
    <h3 class="resize-handle">&#10529</h3>
    <span class="sectionHeaderTitle">freeparty.jpeg</span>
    <div class="sectionHeaderControls">
      <button
        class="fullscreen-button"
        type="button"
        on:click={() => toggleSectionFullscreen("audioGallery")}
        ><h3>&#9974</h3>
      </button>
      <button
        class="close-button"
        type="button"
        on:click={() => loadMenu("audioGallery")}
        ><h3>&#88</h3>
      </button>
    </div>
  </div>
  <div class="audioGalleryContent">
    <img
      src={audioGalleryImages[activeGalleryIndex]}
      alt={`DJing gallery image ${activeGalleryIndex + 1}`}
    />
    <button
      class="audioNav"
      id="galleryBackward"
      type="button"
      on:click={() => changeGalleryImage(-1)}>&lt;&lt;</button
    >
    <button
      class="audioNav"
      id="galleryForward"
      type="button"
      on:click={() => changeGalleryImage(1)}>&gt;&gt;</button
    >
    <div class="audioGalleryCount">
      {activeGalleryIndex + 1} / {audioGalleryImages.length}
    </div>
  </div>
</div>

<div id="cv" class="details">
  <div class="sectionHeader" use:draggable>
    <h3 class="resize-handle">&#10529</h3>
    <span class="sectionHeaderTitle">cv.pdf</span>
    <div class="sectionHeaderControls">
      <button
        class="fullscreen-button"
        type="button"
        on:click={() => toggleSectionFullscreen("cv")}
        ><h3>&#9974</h3>
      </button>
      <button class="close-button" type="button" on:click={() => loadMenu("cv")}
        ><h3>&#88</h3>
      </button>
    </div>
  </div>
  <div class="cvHolder">
    <h1 class="cvHeader">Curriculum Vitae</h1>
    <br /><br />
    <h2 class="cvSubHeader" id="cv-name">Marko Jeremic</h2>
    <br />
    <div class="cvContacts">
      markolemesos@gmail.com | thispage.com | 1108 Boyslton, Boston, MA | <a
        href={resume}
        download>Resume</a
      >
    </div>
    <br />
    <div class="education cvTitle">Education</div>
    <div class="cvContent">
      <div class="cvMiniTitle">Berklee College of Music</div>
      <ul>
        <li>
          Bachelor of Music, Electronic Production and Design, Creative Coding
          2023-2026
        </li>
        <li>3.98 GPA, 6x Dean's List</li>
        <li>World Tour Scholarship</li>
        <li>Max Matthews award recepient</li>
      </ul>
      <br />
      <div class="cvMiniTitle">Stanford University</div>
      <ul>
        <li>
          MA in Music, Science, and Technology, 2026-2028 at the Center for
          Computer Research in Music and Acoustics (CCRMA)
        </li>
      </ul>
    </div>
    <div class="workExperience cvTitle">Work Experience</div>
    <div class="cvContent">
      <div class="cvMiniTitle">
        Resident Assistant, Berklee College of Music
      </div>
      <ul class="cvMiniDesc">
        <li>Resident Assistant for over 30 residents</li>
        <li>Ran the first year abroad living learning community</li>
        <li>Co-program of the year award recipient</li>
      </ul>
      <div class="cvMiniTitle">
        Media Team - AIDA Freediving World Championship 2025
      </div>
      <ul class="cvMiniDesc">
        <li>Edited Videos for opening and closing ceremonies</li>
        <li>
          Controlled stadium cameras for live broadcast and stadium video
          display
        </li>
        <li>
          Wrote custom software for displaying scores and results from the AIDA
          API
        </li>
      </ul>
      <div class="cvMiniTitle">Technology Consultant, SOUNDSCAPE Studios</div>
      <ul class="cvMiniDesc">
        <li>
          Integrated and installed remote recording technology for songwriting
          and sound design
        </li>
        <li>
          Designed database architecture and worked on UI/UX for the companys
          new website using Wix API
        </li>
        <li>Contributed towards sound design for commercials</li>
      </ul>
    </div>
    <div class="workExperience cvTitle">Installations</div>
    <br />
    <div class="cvMiniTitle">
      2025, fusionem artis - Synthember 2025, installed at New Alliance Gallery
    </div>
    <div class="cvMiniTitle">
      2024, Facing West Shadows - Lysistrata, audio production,mixing and
      recording
    </div>
    <br />
    <div class="cvTitle">Visual Design</div>
    <br />
    <div class="cvMiniTitle">
      My words don't define me, my actions do - 2025
    </div>
    <div class="cvMiniTitle">
      Live visual design - .ivy, T-rey Spitz, and others
    </div>
    <div class="cvTitle">Software</div>
    <br />
    <div class="cvMiniTitle">
      2025, HydraRenderer - Tool for rendering Hydra visuals faster than real
      time
    </div>
    <div class="cvMiniTitle">
      2025, EMC, Coral Mission - Worked with Future Worlds to create a custom
      vignette for Earth Mission Control
    </div>
    <div class="cvMiniTitle">
      2024, Audio2MIDI - Real time audio to MIDI conversion tool
    </div>
    <div class="cvMiniTitle">
      2024, MIDISampler - Multi-FX sampler built in Csound
    </div>
    <br />
    <div class="cvTitle">Publications</div>
    <div class="cvMiniTitle">
      <br />
      <a href="https://nime.org/proc/nime2026_163/index.html"
        >e-baton: Recognising Conducting Gestures with Machine Learning, NIME
        2026</a
      >
    </div>
  </div>
</div>
<button class="close-button" type="button" on:click={() => loadMenu("cv")}
  ><div>X</div>
</button>
<div id="about" class="details">
  <div class="sectionHeader" use:draggable>
    <h3 class="resize-handle">&#10529</h3>
    <span class="sectionHeaderTitle">aboutme.md</span>

    <div class="sectionHeaderControls">
      <button
        class="fullscreen-button"
        type="button"
        on:click={() => toggleSectionFullscreen("about")}
        ><h3>&#9974</h3>
      </button>
      <button
        class="close-button"
        type="button"
        on:click={() => loadMenu("about")}
        ><h3>&#88</h3>
      </button>
    </div>
  </div>
  <h1 style="text-align: center;">About Me!</h1>
  <div class="sectionText">
    My name is Marko Jeremic, I am a Musician, programmer, and creator,
    dedicated to exploring the space of how we interact with music. Growing up
    in Limassol, Cyprus with Serbian parents showed me the challenges of
    communication from a young age, where I found myself turning to the
    saxophone. Since then, I’ve found myself exploring different ways in which
    we can communicate with one another, through music and other various forms
    of art. Regardless of language and ability, people have always found ways to
    express their ideas, and I hope to further explore this through my practice
    based research.<br /> Through my background in programming and engineering,
    I hope to work towards creating new tools for artists to use to express
    their artistic ideas. A city such as Boston, where I am currently based out
    of, has so much diversity, so many stories to tell, yet people are so often
    forced down the same path of creation, regardless of what they want to
    express. Through discussions with others, I hope to learn what creatives
    need for their art, as well as how we can make art more accessible to access
    and appreciate.<br /> My current work is focused on physical computing and
    installation design. Developments in machine learning have made it
    increasingly viable for musical applications, providing new ways to
    translate gestures and movement into data we can use to control electronic
    devices. Alongside this I am actively designing visuals for various artists
    in the area, as well as working on audio implementation for a newly
    announced indie game, Grimorium.<br /> Sound theorist Salomé Voeglin’s discussion
    on an approach to practice-based research has inspired me to engage with my work
    through both a creative and technical lens, using these technologies to explore
    the creative and expressive possibilities. Voegelin also emphasizes the importance
    of creation through necessity, and that oftentimes the invention of new technologies
    is inspired by someone’s need to express their idea. With this mindset I hope
    to not lose sight of the creative vision through the technological processes
    required to each and express it.
  </div>
</div>
<div id="software2" class="details">
  <div class="sectionHeader" use:draggable>
    <h3 class="resize-handle">&#10529</h3>
    <span class="sectionHeaderTitle">software.app</span>

    <div class="sectionHeaderControls">
      <button
        class="fullscreen-button"
        type="button"
        on:click={() => toggleSectionFullscreen("software2")}
        ><h3>&#9974</h3>
      </button>
      <button
        class="close-button"
        type="button"
        on:click={() => loadMenu("software2")}
        ><h3>&#88</h3>
      </button>
    </div>
  </div>
  <div class="softwareMenu">
    {#each softwareProjects as project, index}
      <button
        type="button"
        class:active={activeSoftwareIndex === index}
        on:click={() => (activeSoftwareIndex = index)}
      >
        {project.title}
      </button>
    {/each}
  </div>
</div>
<div id="software1" class="details">
  <div class="sectionHeader" use:draggable>
    <h3 class="resize-handle">&#10529</h3>
    <span class="sectionHeaderTitle">softwares.exe</span>

    <div class="sectionHeaderControls">
      <button
        class="fullscreen-button"
        type="button"
        on:click={() => toggleSectionFullscreen("software1")}
        ><h3>&#9974</h3>
      </button>
      <button
        class="close-button"
        type="button"
        on:click={() => loadMenu("software1")}
        ><h3>&#88</h3>
      </button>
    </div>
  </div>
  <div class="softwareProject">
    <h1>{softwareProjects[activeSoftwareIndex].title}</h1>
    {#if softwareProjects[activeSoftwareIndex].image}
      <img
        class="softwareProjectImage"
        src={softwareProjects[activeSoftwareIndex].image}
        alt={softwareProjects[activeSoftwareIndex].title}
      />
    {/if}
    <div class="sectionText softwareProjectDescription">
      {softwareProjects[activeSoftwareIndex].description}
      {#if softwareProjects[activeSoftwareIndex].link}
        <a
          href={softwareProjects[activeSoftwareIndex].link}
          target="_blank"
          rel="noreferrer"
        >
          {softwareProjects[activeSoftwareIndex].linkLabel}
        </a>
      {/if}
      {#if softwareProjects[activeSoftwareIndex].items}
        <ul>
          {#each softwareProjects[activeSoftwareIndex].items as item}
            <li>
              {#if typeof item === "string"}
                {item}
              {:else}
                {item.text}
                {#if item.link}
                  <a href={item.link} target="_blank" rel="noreferrer">
                    {item.linkLabel || "Read more"}
                  </a>
                {/if}
              {/if}
            </li>
          {/each}
        </ul>
      {/if}
    </div>
    {#if softwareProjects[activeSoftwareIndex].video}
      <iframe
        title={softwareProjects[activeSoftwareIndex].title}
        src={softwareProjects[activeSoftwareIndex].video}
        frameborder="0"
        allowfullscreen
      ></iframe>
    {/if}
    <div class="softwareNav">
      <button type="button" on:click={() => changeSoftware(-1)}>&lt;&lt;</button
      >
      <span>{activeSoftwareIndex + 1} / {softwareProjects.length}</span>
      <button type="button" on:click={() => changeSoftware(1)}>&gt;&gt;</button>
    </div>
  </div>
</div>
<div id="backgroundVideo" class:black-mode={!showBackgroundVideo}>
  {#if showBackgroundVideo}
    <canvas bind:this={hydraCanvas}></canvas>
  {/if}
</div>

<div class="mobile-blocker" role="dialog" aria-modal="true">
  <h1>Desktop only</h1>
  <p>
    hi sorry the mobile version is not complete! please check this awesome
    website out on a computer/larger screen &lt;/3 &lt;/3
  </p>
</div>
