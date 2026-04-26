<script>
  import backgroundVideo from "./assets/video.mp4";
  import resume from "./assets/resume_jeremic.pdf";
  import testSong from "./assets/test.wav";
  import testSong2 from "./assets/touchdesigning.mp3";
  import { onMount } from "svelte";

  onMount(() => {
    initAudioPlayer();
  });
  const tabs = document.getElementsByClassName("sectionHeader");

  let currentAudio = null;
  let isPlaying = false;

  function playAudio() {
    const audioElement = /** @type {HTMLAudioElement | null} */ (
      document.getElementById("audioPlayer")
    );
    const playButton = document.getElementById("playButton");
    const progressSlider = document.getElementById("progressSlider");

    if (!audioElement || !playButton || !progressSlider) return;

    if (isPlaying) {
      audioElement.pause();
      playButton.textContent = "Play";
      isPlaying = false;
    } else {
      audioElement.play();
      playButton.textContent = "Pause";
      isPlaying = true;
    }
  }

  function updateSlider() {
    const audioElement = /** @type {HTMLAudioElement | null} */ (
      document.querySelector("#audioPlayer")
    );
    const progressSlider = /** @type {HTMLInputElement | null} */ (
      document.getElementById("progressSlider")
    );

    if (!audioElement || !progressSlider) return;

    const percent = (audioElement.currentTime / audioElement.duration) * 100;
    progressSlider.value = percent.toString();
    progressSlider.style.setProperty("--progress", percent + "%");
  }

  function scrubAudio(e) {
    const audioElement = /** @type {HTMLAudioElement | null} */ (
      document.querySelector("#audioPlayer")
    );
    if (!audioElement) return;

    const progressSlider = e.target;
    const newTime = (progressSlider.value / 100) * audioElement.duration;
    audioElement.currentTime = newTime;
  }

  function initAudioPlayer() {
    const audioElement = document.getElementById("audioPlayer");
    if (!audioElement) return;

    audioElement.addEventListener("timeupdate", updateSlider);
    audioElement.addEventListener("ended", () => {
      isPlaying = false;
      const playButton = document.getElementById("playButton");
      if (playButton) playButton.textContent = "Play";
    });
  }

  const videos = {
    video1: {
      url: "https://youtube.com/embed/kVsvYNYZnN0",
      description: "short film......",
    },
    video2: {
      url: "https://youtube.com/embed/8ijJ3A1Hv_4",
      description: "another video...",
    },
  };

  const audio = {
    audio1: {
      url: testSong,
      description: "Audio....1.....",
    },
    audio2: {
      url: testSong2,
      description: "Audio....2.....",
    },
  };
  let activeVideo = "video1";
  let activeIndex = 1;

  function changeVideo(direction) {
    const videoHolder = document.getElementById("video2");
    activeIndex = ((activeIndex - 1 + direction) % 2) + 1;
    activeVideo = `video${activeIndex}`;
    const videoDisplay = /** @type {HTMLIFrameElement | null} */ (
      document.querySelector("#videoPlayer")
    );
    const videoDesc = document.getElementById("videoDescription");
    if (!videoDisplay || !videoDesc) return;
    videoDisplay.src = videos[activeVideo].url;
    videoDesc.textContent = videos[activeVideo].description;
  }

  function changeAudio(direction) {
    activeIndex = ((activeIndex - 1 + direction) % 2) + 1;
    const activeAudio = `audio${activeIndex}`;
    const audioPlayer = /** @type {HTMLAudioElement | null} */ (
      document.querySelector("#audioPlayer")
    );
    const audioDesc = document.getElementById("audioDescription");
    if (!audioPlayer || !audioDesc) return;
    audioPlayer.src = audio[activeAudio].url;
    audioDesc.textContent = audio[activeAudio].description;
    playAudio();
  }
  function draggable(node) {
    let x = 0,
      y = 0;

    function handleMouseDown(e) {
      const rect = node.parentElement.getBoundingClientRect();
      document.body.style.userSelect = "none";
      x = e.clientX - rect.left;
      y = e.clientY - rect.top;

      function handleMouseMove(moveEvent) {
        const parent = node.parentElement;
        parent.style.left = moveEvent.clientX - x + "px";
        parent.style.top = moveEvent.clientY - y + "px";
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

        parent.style.width = width + "px";
        parent.style.height = height + "px";
        parent.style.left = startLeft + deltaX + "px";
        parent.style.top = startTop + deltaY + "px";
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
    software: {
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
    targetSection.style.opacity = "1";
    targetSection.style.top = layout.top;
    targetSection.style.left = layout.left;
    targetSection.style.width = layout.width;
    targetSection.style.height = layout.height;
    targetSection.style.overflow = layout.overflow;

    setTimeout(() => {
      targetSection.style.transition = "none";
    }, 500);
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
    targetSection.style.transition =
      "height 0.5s ease-in-out, width 0.5s ease-in-out, top 0.5s ease-in-out, left 0.5s ease-in-out";

    if (flag == layout.fullscreen) {
      applySectionLayout(targetSection, layout.fullscreen);
    } else {
      applySectionLayout(targetSection, layout.open);
    }

    setTimeout(() => {
      targetSection.style.transition = "none";
    }, 500);

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

    targetSection.style.top = layout.closed.top;
    targetSection.style.left = layout.closed.left;
    targetSection.style.width = layout.closed.width;
    targetSection.style.height = layout.closed.height;
    targetSection.style.opacity = "0";
    targetSection.style.overflow = layout.closed.overflow;
    targetSection.classList.remove("open");
    targetSection.classList.remove("fullscreen");

    if (closeButton) {
      closeButton.style.opacity = "0";
    }

    setBackgroundToggleVisible(true);
  }
</script>

<label class="bg-toggle" for="bg-video-toggle">
  <span>Background</span>
  <input
    class="bg-toggle-input"
    id="bg-video-toggle"
    type="checkbox"
    bind:checked={showBackgroundVideo}
    aria-label="Toggle background video"
  />
  <span class="bg-toggle-slider" aria-hidden="true"></span>
</label>

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
    ><h2 class="loaded">Music</h2></button
  >
  <button type="button" on:click={() => loadSection("cv")}
    ><h2 class="loaded">CV</h2></button
  >
</div>

<div id="video1" class="details">
  <div class="sectionHeader" use:draggable>
    <div class="sectionHeaderControls">
      <button
        class="close-button"
        type="button"
        on:click={() => loadMenu("video1")}
      >
        <h3>X</h3>
      </button>
    </div>
  </div>
  <h1>Video Content 1!</h1>
  <div id="videoDescription">
    Talk about the short film blah blah blah!!!!!!!
  </div>
  <div class="resize-handle"></div>
  <button class="videoNav" id="backward" on:click={() => changeVideo(-1)}>
    &lt;&lt;
  </button>
  <button class="videoNav" id="forward" on:click={() => changeVideo(1)}>
    &gt;&gt;
  </button>
</div>

<div id="video2" class="details">
  <div class="sectionHeader" use:draggable>
    <div class="sectionHeaderControls">
      <button
        class="close-button"
        type="button"
        on:click={() => loadMenu("video2")}
      >
        <h3>X</h3>
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
  <div class="resize-handle"></div>
</div>

<div id="audio1" class="details">
  <div class="sectionHeader" use:draggable>
    <div class="sectionHeaderControls">
      <button
        class="close-button"
        type="button"
        on:click={() => loadMenu("audio")}
      >
        <h3>X</h3>
      </button>
    </div>
    <div id="audioDescription">Audio....1....</div>
  </div>
  <div class="resize-handle"></div>
</div>
<div id="audio2" class="details">
  <div>
    <audio src={testSong} preload="metadata" id="audioPlayer"></audio>
    <button id="playButton" on:click={playAudio}>Play</button>
    <div id="transportControls">
      <button class="audioTransport" on:click={() => changeAudio(-1)}>
        &lt;&lt;
      </button>
      <input
        id="progressSlider"
        type="range"
        min="0"
        max="100"
        value="0"
        on:change={scrubAudio}
        on:input={scrubAudio}
      />
      <button class="audioTransport" on:click={() => changeAudio(1)}>
        &gt;&gt;
      </button>
    </div>
  </div>
</div>
<div id="cv" class="details">
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
        <li>3.99 GPA, 5x Dean's List</li>
        <li>World Tour Scholarship</li>
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
      <div class="cvMiniTitle">Booking Assistant, Berklee College of Music</div>
      <ul class="cvMiniDesc">
        <li>Assisted students with booking studio time for projects</li>
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
    <div class="cvMiniTitle">2025, Graffiti project lol</div>
    <div class="cvMiniTitle">
      2024, Facing West Shadows - Lysistrata, audio production,mixing and
      recording
    </div>
    <br />
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
  </div>
  <button class="close-button" type="button" on:click={() => loadMenu("cv")}
    ><div>X</div>
  </button>
</div>
<div id="about" class="details">
  <div class="sectionHeader" use:draggable>
    <div class="sectionHeaderControls">
      <button
        class="fullscreen-button"
        type="button"
        on:click={() => toggleSectionFullscreen("about")}
        ><h3>□</h3>
      </button>
      <button
        class="close-button"
        type="button"
        on:click={() => loadMenu("about")}
        ><h3>X</h3>
      </button>
    </div>
  </div>
  <h1>About Me!</h1>
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
  <div class="resize-handle"></div>
</div>
<div id="backgroundVideo" class:black-mode={!showBackgroundVideo}>
  {#if showBackgroundVideo}
    <video autoplay muted loop playsinline>
      <source src={backgroundVideo} type="video/mp4" />
    </video>
  {/if}
</div>
