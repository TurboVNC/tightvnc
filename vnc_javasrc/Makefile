#
# Making the VNC applet.
#

CP = cp
JC = javac
JAR = jar
ARCHIVE = VncViewer.jar
MANIFEST = MANIFEST.MF
PAGES = index.vnc
INSTALL_DIR = /usr/local/vnc/classes

CLASSES = VncViewer.class RfbProto.class AuthPanel.class VncCanvas.class \
	  OptionsFrame.class ClipboardFrame.class ButtonPanel.class \
	  DesCipher.class RecordingFrame.class SessionRecorder.class \
	  SocketFactory.class HTTPConnectSocketFactory.class \
	  HTTPConnectSocket.class ReloginPanel.class

SOURCES = VncViewer.java RfbProto.java AuthPanel.java VncCanvas.java \
	  OptionsFrame.java ClipboardFrame.java ButtonPanel.java \
	  DesCipher.java RecordingFrame.java SessionRecorder.java \
	  SocketFactory.java HTTPConnectSocketFactory.java \
	  HTTPConnectSocket.java ReloginPanel.java

all: $(CLASSES) $(ARCHIVE)

$(CLASSES): $(SOURCES)
	$(JC) -O $(SOURCES)

$(ARCHIVE): $(CLASSES) $(MANIFEST)
	$(JAR) cfm $(ARCHIVE) $(MANIFEST) $(CLASSES)

install: $(CLASSES) $(ARCHIVE)
	$(CP) $(CLASSES) $(ARCHIVE) $(PAGES) $(INSTALL_DIR)

export:: $(CLASSES) $(ARCHIVE) $(PAGES)
	@$(ExportJavaClasses)

clean::
	$(RM) *.class *.jar
