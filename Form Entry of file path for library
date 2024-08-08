from flask import Flask, request, render_template
import xml.etree.ElementTree as ET
import os

app = Flask(__name__)

@app.route('/', methods=['GET', 'POST'])
def index():
    if request.method == 'POST':
        file_path = request.form['file_path']
        volume_adjustment = int(request.form['volume_adjustment'])
        try:
            xml_tree = ET.parse(file_path)
            update_volume_adjustment(xml_tree, volume_adjustment)
            xml_tree.write('updated_' + os.path.basename(file_path))
            return 'Volume adjustment updated successfully!'
        except ET.ParseError as e:
            return f'Error parsing XML file: {e}'
    return render_template('index.html')

def update_volume_adjustment(xml_tree, volume_adjustment):
    """Update the volume adjustment for each track in the playlist"""
    playlist_root = xml_tree.getroot()
    tracks = playlist_root.findall('.//dict[@key="Track ID"]')
    for track in tracks:
        track_id = track.find('integer').text
        volume_adjustment_element = playlist_root.find(f'.//dict[@key="Track ID"][integer={track_id}]/dict[@key="Volume Adjustment"]')
        if volume_adjustment_element is not None:
            volume_adjustment_element.find('integer').text = str(volume_adjustment)

if __name__ == '__main__':
    app.run(debug=True)
